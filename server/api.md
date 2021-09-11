This is the API that is used by the Playdate console for things like fetching game updates, scoreboards, player details, etc. It is available under `https://play.date/api/v2`. All API endpoints require [auth headers](#auth-headers).

This list of endpoints was obtained by decompiling the Playdate Simulator app. Some haven't actually been seen in use, so they are only partially documented.

## Endpoints

| Method | Path |
|:-|:-|
| `POST` | [`/auth_echo/`](#post-auth_echo) |
| `GET`  | [`/player/`](#get-player) |
| `GET`  | [`/player/:playerId/`](#get-playerplayerid) |
| `POST` | `/player/avatar/` |
| `GET`  | `/games/scheduled/` |
| `GET`  | `/games/testing/` |
| `GET`  | `/games/user/` |
| `GET`  | `/games/purchased/` |
| `GET`  | `/games/system/` |
| `GET`  | `/games/:bundleId/latest_build/` |
| `GET`  | `/games/:bundleId/boards/` |
| `GET`  | `/games/:bundleId/boards/:unknown/` |
| `POST` | `/games/:bundleId/boards/:unknown/` |
| `GET`  | `/device/settings` |
| `POST` | `/device/register/:serialNumber` |

### POST /auth_echo

Seems to just return whatever JSON body is sent to it.

### GET /player

Returns the player profile for the user that owns the current access token.

### GET /player/:playerId

Same as `/player`, but gets the player profile for another user, given their [Player ID](#player-id).

## Auth Headers

All routes require a basic authorization token sent via a HTTP header. If you have a developer account on [play.date](//play.date), you can generate an access token by going to `https://play.date/players/account/` and clicking 'register simulator'. It looks as though you're allowed to register up to 5 simulators at one time.

| Header | Value |
|:-|:-|
| `Authorization` | `Token ` followed by your authorization token |

## Player ID

Player IDs seem to use the `DCE 1.1, ISO/IEC 11578:1996` variant of UUID v4, with dashes separating each section, e.g `XXXXXXXX-XXXX-4XXX-8XXX-XXXXXXXXXXXX`.

If you're signed into your account on your Playdate, then your personal Player ID can be found in `cachedAccountInfo.json` under `com.panic.settings` on your Playdate's data partition.