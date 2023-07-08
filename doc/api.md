# BBScore API

## Error Response

When responding with a 4xx or 5xx response code to any API call, the server should return error
information in this format instead of returning the normal response for that call.

    {
        "error": "Invalid name",
        "detail": "name must be non-empty"
    }

* `error`: The user-readable text of the error.
* `detail`: More specific text describing what exactly went wrong.

## Tournaments (/api/tournaments)

### List tournaments (GET /api/tournaments)

**Requires authentication.**
Retrieves the currently logged in TO's tournament list.

#### Status codes

* **200**: Successfully listed tournaments.
* **401**: User is not logged in.
* **500**: Server failed to look up tournaments for any other reason.

#### Response

    {
        "tournaments": [{
            "id": "1234567890abcdef",
            "name": "Tournament Name"
            "date": {
              year: 2012
              month: 12
              date: 1
            }
        }]
    }

* `tournaments`: Array of objects. The list of tournaments owned by this user.
    * `id`: String. An opaque, unique ID used to access the details about this tournament.
    * `name`: String. A user-specified and user-readable name suitable for display in a tournament
      list.
    * `date`: Object. Date the tournament was run.
 

### Create tournament (POST /api/tournaments)

**Requires authentication.**
Creates a new tournament owned by the currently logged in director.

#### Request

    {
        "name": "Tournament Name",
        "date": {
              year: 2012
              month: 12
              date: 1
        },
        "players": [{
            "naf_name": "Giraffe",
            "naf_number": 123
        }],
        "allow_mid_round_results": True,
        "admins": [{
          "email": "email@email.com"
        }]
    }

* `name`: String. A user-specified and user-readable name suitable for display in a tournament list.
  Required.
* `date`: Object. Date the tournament was run.
* `players`: Integer. The number of pairs (teams) to play in this tournament. Must have length more than
  than 1. Required.
* `players`: List of objects. More information about the players. There should be at most
  two players for the same `pair_no`. Optional.
    * `naf_number`: Integer. The pair this player belongs to. Must be between 0 and `no_pairs`. Required.
    * `naf_name`: String. User-readable name for the player. Required.
* `allow_mid_round_results`: Boolean. If true, allow players to see results as they come in in the middle of
      the round as long as they have finished their own game.
* `admins`: List of people who have admin priveledges in this tournament.
    * `email`: String. Email address of the admin.

      
#### Status codes

* **201**: The tournament was successfully created.
* **400**: One or more required fields were not specified, or failed validation.
* **401**: User is not logged in.
* **500**: Server failed to create a tournament for any other reason.
 
#### Response

    {
        "id": "1234567890abcdef"
    }

* `id`: String. An opaque, unique ID used to access the details about the newly created tournament.
