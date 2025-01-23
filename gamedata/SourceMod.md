**Reaaliaikaisen datan kerääminen SourceModilla:**
	
- Yhteys pelipalvelimen ja Unityn välille.
- Tiedot voidaan lähettää Unityyn esim. WebSocketien tai HTTP (REST API).
	- Halutaan varmaan käyttää WebSocketia???
- Peliäänet tapahtumien mukaan, samoin kuin animointi???
	- Äänitiedostot saadaan pelin .vpk-tiedostoista. 
- MetaMod: Source toimii laajennusalustana, joka mahdollistaa SourceModin käytön.
- Mahdollistaa sovelluksen käyttämisen ilman peliä.

**CS2-serverin luominen:**

- https://developer.valvesoftware.com/wiki/Counter-Strike_2/Dedicated_Servers

- Lataa ja asenna **SteamCMD**: https://developer.valvesoftware.com/wiki/SteamCMD
- Ohjeet löytyvät linkistä.
- Löytyy myös **SteamCMD GUI:** https://github.com/AndrSator/SteamCMD-GUI
- Docker???
-  Avaa SteamCMD ja kirjaudu sisään komennolla:
		`login anonymous`
- Lataa CS2-pelipalvelin komennolla:
		`app_update 740 validate`
- Luo palvelimen käynnistystiedosto:
	- Windows: ==start.bat==
	- Linux: ==start.sh==
		- Esimerkki käynnistyskomennosta:
				`srcds.exe -game csgo -console -usercon +map de_mirage +maxplayers 16`
- Käynnistä pelipalvelin.

**SourceModin ja MetaModin asennus:**

- **Metamod lataussivu:** https://www.sourcemm.net/downloads.php
- **SourceMod lataussivu:** https://www.sourcemod.net/downloads.php
- **Socket Extension SourceModille:** https://forums.alliedmods.net/showthread.php?t=67640 
	- Mahdollistaa WebSocketin käytön SourceModilla. 

**Socket Extension:**

-  Lataa ja asenna Socket Extension SourceModille. Tämä mahdollistaa TCP- ja WebSocket-yhteyksien käytön.
-  Lisää **.smx-tiedosto** SourceModin `plugins/`-kansioon ja varmista, että plugin ladataan ilman virheitä.



#### **Esimerkkejä:***

**Esimerkki SourceMod-pluginista WebSocketilla:**

	#include <sourcemod>
	#include <socket>
	
	Handle g_WebSocket = INVALID_HANDLE;
	
	// Pluginin lataus
	public void OnPluginStart()
	{
	    PrintToServer("WebSocket Plugin loaded!");
	
	    // Yhdistä Unity-sovellukseen
	    g_WebSocket = SocketCreate(SocketType_Stream, OnWebSocketConnect);
	    if (g_WebSocket != INVALID_HANDLE)
	    {
	        SocketConnect(g_WebSocket, "127.0.0.1", 5001); // Unityn WebSocket-palvelin
	    }
	    else
	    {
	        PrintToServer("Failed to create WebSocket handle.");
	    }
	}
	
	// WebSocket-yhteyden muodostus
	public void OnWebSocketConnect(Handle socket, SocketError error)
	{
	    if (error == SocketError_None)
	    {
	        PrintToServer("WebSocket connected to Unity!");
	    }
	    else
	    {
	        PrintToServer("WebSocket connection failed: %d", error);
	        CloseSocket();
	    }
	}
	
	// Lähetä dataa Unityyn
	void SendWebSocketData(const char[] data)
	{
	    if (g_WebSocket != INVALID_HANDLE)
	    {
	        SocketSend(g_WebSocket, data, strlen(data));
	        PrintToServer("Sent data: %s", data);
	    }
	    else
	    {
	        PrintToServer("WebSocket is not connected.");
	    }
	}
	
	// Sulje WebSocket-yhteys
	void CloseSocket()
	{
	    if (g_WebSocket != INVALID_HANDLE)
	    {
	        SocketDestroy(g_WebSocket);
	        g_WebSocket = INVALID_HANDLE;
	        PrintToServer("WebSocket connection closed.");
	    }
	}

	**Pluginin toiminta:**
	
	1. Yhdistää Unity-sovellukseen, joka kuuntelee osoitteessa `127.0.0.1:5001`.
	2. Käyttää `SendWebSocketData`-funktiota tietojen lähettämiseen reaaliajassa.


**Esimerkki WebSocket-palvelimen toteuttamisesta Unityssä:**
	
	using System;
	using System.Net;
	using System.Net.Sockets;
	using System.Text;
	using System.Threading;
	using UnityEngine;
	
	public class WebSocketServer : MonoBehaviour
	{
	    private TcpListener server;
	    private Thread serverThread;
	
	    void Start()
	    {
	        serverThread = new Thread(StartServer);
	        serverThread.IsBackground = true;
	        serverThread.Start();
	    }
	
	    private void StartServer()
	    {
	        try
	        {
	            server = new TcpListener(IPAddress.Parse("127.0.0.1"), 5001);
	            server.Start();
	            Debug.Log("WebSocket Server started on port 5001.");
	
	            while (true)
	            {
	                TcpClient client = server.AcceptTcpClient();
	                NetworkStream stream = client.GetStream();
	
	                byte[] buffer = new byte[1024];
	                int bytesRead = stream.Read(buffer, 0, buffer.Length);
	                string message = Encoding.UTF8.GetString(buffer, 0, bytesRead);
	
	                Debug.Log("Received: " + message);
	
	                // Lähetä vastaus tarvittaessa
	                string response = "Message received: " + message;
	                byte[] responseBytes = Encoding.UTF8.GetBytes(response);
	                stream.Write(responseBytes, 0, responseBytes.Length);
	            }
	        }
	        catch (Exception e)
	        {
	            Debug.LogError("Error in WebSocket Server: " + e.Message);
	        }
	    }
	
	    private void OnApplicationQuit()
	    {
	        if (server != null)
	        {
	            server.Stop();
	        }
	    }
	}


**SourceMod lähettää tiedon näin:**

	SendWebSocketData("{\"event\":\"jump\",\"player\":\"PlayerName\"}");


**SteamID tunniste SourceModissa:**

	char steamId[32];
	GetClientAuthId(client, AuthId_Steam2, steamId, sizeof(steamId));
	PrintToServer("Player SteamID: %s", steamId);


**Esimerkki äänitapahtumien lähettämisestä Unityyn:**

	public void OnPluginStart()
	{
	    HookEvent("weapon_fire", OnWeaponFire);
	}
	
	public void OnWeaponFire(Event event, const char[] name, bool dontBroadcast)
	{
	    int client = GetClientOfUserId(event.GetInt("userid"));
	    char weapon[64];
	    event.GetString("weapon", weapon, sizeof(weapon));
	
	    char jsonData[128];
	    Format(jsonData, sizeof(jsonData), "{\"event\":\"weapon_fire\", \"player_id\":\"%d\", \"weapon\":\"%s\"}", client, weapon);
	    SendWebSocketData(jsonData); // Lähetä Unitylle
	}


**Esimerkki äänitapahtuman vastaanottamisesta Unityssä:**

	public class SoundManager : MonoBehaviour
	{
	    public AudioClip pistolSound;
	    public AudioClip rifleSound;
	
	    public void PlaySound(string weapon)
	    {
	        AudioSource audioSource = gameObject.AddComponent<AudioSource>();
	        switch (weapon)
	        {
	            case "pistol":
	                audioSource.clip = pistolSound;
	                break;
	            case "rifle":
	                audioSource.clip = rifleSound;
	                break;
	        }
	        audioSource.Play();
	    }
	}


