# Gamehearth catalog

The catalog of games for Gamehearth, a self-hostable game platform. This repo contains only the index `catalog.json`: per game the name, description and a link to the source code, and per version the player count, roles, license, size, download URL and SHA-256 checksum. The game code lives in each game's own repo, and the packages are attached to its releases.

## Using it on a server

In the admin view of the Gamehearth server, under "Catalogs" ("Kataloge"), enter the URL (or take the suggestion "Add Gamehearth catalog" ("Gamehearth-Katalog hinzufügen")):

```
https://raw.githubusercontent.com/hearthbit/gamehearth-catalog/main/catalog.json
```

Adding a catalog means trusting it: its games run on the server and in the browsers of the other players. The server checks every package against the checksum in the catalog and unpacks it strictly; for games with rules on the server it also loads them into its sandbox and plays a trial match before it installs the game. Single-player games without rules on the server (like Mauerbrecher) run entirely in the browser; the server only keeps saves and best scores. It reloads the catalog once a day and shows new versions in the admin view; nothing is installed until someone clicks.

## Games

| Game | Repo |
|---|---|
| Mau-Mau | [hearthbit/gamehearth-mau-mau](https://github.com/hearthbit/gamehearth-mau-mau) |
| Farbfelder | [hearthbit/gamehearth-farbfelder](https://github.com/hearthbit/gamehearth-farbfelder) |
| Mauerbrecher | [hearthbit/gamehearth-mauerbrecher](https://github.com/hearthbit/gamehearth-mauerbrecher) |
| Angelteich | [hearthbit/gamehearth-angelteich](https://github.com/hearthbit/gamehearth-angelteich) |
| Kritzelkiste | [hearthbit/gamehearth-kritzelkiste](https://github.com/hearthbit/gamehearth-kritzelkiste) |

## Adding a version

You need Node 22. The package must already be attached as a release asset in the game's repo (the CI there builds it with `gamehearth pack`).

```bash
npm install
npm run add -- https://github.com/hearthbit/gamehearth-mau-mau/releases/download/v0.5.1/mau-mau-0.5.1.zip
npm run check
```

`npm run add` downloads the package, checks it like the server does and enters version, size and checksum; name, description and repo link come from the manifest of the newest version. A version that is already listed with a different checksum is only overwritten with `--replace`; a new version number is better. `npm run check` checks the shape of the file and downloads every listed package once (the CI does this too, on every push and once a week).

Older versions stay listed so that a server can roll back a broken update.

## License

The index is licensed under CC0 (see [LICENSE](LICENSE)). Every game has its own license; it is listed in the catalog and in the game's manifest.
