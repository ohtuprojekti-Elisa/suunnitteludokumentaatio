# JSON-datan määrittely



```json
{
    "interval": 15.625,
    "total_ticks": 100000,
    "map_name": "dust2",
    "ticks": [
        {
            "tick": 1,
            "round_number": 1,
            "ct_wins": 1,
            "t_wins": 0,
            "who_won": "null",
            "players": [
                {
                    "id": 1231241,
                    "name": "cs_proxd",
                    "team": "ct",
                    "hp": 100,
                    "weapon": {
                        "name": "AWP",
                        "type": "primary"
                    }
                    "grenades": ["flash", "he"],
                    "armor": 100,
                    "helmet": true,
                    "money_sum": 1000,
                    "kills": 2,
                    "deaths": 3,
                    "assists": 3,
                    "kdr": 1.5,
                    "adr": 170,
                    "dmg": 300,
                    "coords": {
                        "x": 1230.0,
                        "y": 200.0,
                        "z": 10.0
                    },
                    "yaw": 100,
                    "ducked": false,
                    "is_airborne": false,
                    "has_bomb": false,
                    "has_bomb_defuse_kit": true,
                    "planting": false,
                    "defusing": false
                }
            ],
            "grenades": {
                "grenades_in_game": true,
                "active_grenades": [
                    {
                        "type": "null",
                        "owner": "null",
                        "trajectory": "null"
                    },
                    {
                        "type": "flash",
                        "owner": "cs_proxd",
                        "trajectory": "example_trajectory"
                    }
                ]
            },
            "bomb": {
                "owner": "null",
                "position": {
                    "x": 123.0,
                    "y": 10.0,
                    "z": 120.0
                }
                "site": "null",
                "who_is_planting": "null",
                "is_being_planted": false,
                "is_planted": false,
                "is_defused": false,
                "detonation_timer": "null",
                "defuse_timer": "null",
                "is_being_defused": false,
                "who_is_defusing": "null",
            }
        }
    ]
}
```
