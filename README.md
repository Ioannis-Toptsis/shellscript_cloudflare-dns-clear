# Cloudflare DNS Clear

**[Deutsch](#deutsch) | [English](#english)**

---

<a id="deutsch"></a>
## 🇩🇪 Deutsch

Ein simples Bash-Skript, das **alle DNS-Einträge** einer Cloudflare-Zone über die Cloudflare API v4 abruft und anschließend vollständig löscht.

> ⚠️ **Achtung – destruktive Aktion!**
> Dieses Skript löscht **ausnahmslos alle** DNS-Records der angegebenen Zone, ohne Rückfrage und ohne Möglichkeit einer automatischen Wiederherstellung. Setze es nur ein, wenn du dir absolut sicher bist, dass du die komplette DNS-Konfiguration der Zone verwerfen willst. Es wird dringend empfohlen, vorher ein Backup der Zone zu exportieren (siehe [Backup erstellen](#backup-erstellen)).

### Funktionsweise

1. Das Skript ruft über `GET /zones/{zone_id}/dns_records` bis zu 500 DNS-Records der Zone ab.
2. Mit `jq` werden aus der Antwort alle Record-IDs extrahiert.
3. Für jede ID wird anschließend `DELETE /zones/{zone_id}/dns_records/{id}` aufgerufen, wodurch der jeweilige Eintrag entfernt wird.

### Voraussetzungen

- Bash (Linux, macOS oder WSL/Git Bash unter Windows)
- [`curl`](https://curl.se/)
- [`jq`](https://stedolan.github.io/jq/) zum Parsen der JSON-Antworten
- Ein Cloudflare-Account mit:
  - Zone ID der betroffenen Domain
  - Account-E-Mail-Adresse
  - Global API Key (`X-Auth-Key`)

### Installation

```bash
git clone <repository-url>
cd shellscript_cloudflare-dns-clear
chmod +x script.sh
```

### Konfiguration

Öffne `script.sh` und trage oben die drei erforderlichen Werte ein:

```bash
ZONE_ID=deine_zone_id
EMAIL=deine@email.de
KEY=dein_globaler_api_key
```

| Variable  | Beschreibung                                              | Fundort in Cloudflare                                   |
|-----------|-------------------------------------------------------------|-----------------------------------------------------------|
| `ZONE_ID` | ID der Zone (Domain), deren DNS-Einträge gelöscht werden    | Dashboard → Domain → Overview (rechte Seitenleiste)        |
| `EMAIL`   | E-Mail-Adresse des Cloudflare-Accounts                       | Account-Einstellungen                                      |
| `KEY`     | Globaler API Key                                             | Mein Profil → API Tokens → Global API Key                  |

### Verwendung

```bash
./script.sh
```

Das Skript läuft ohne weitere Rückfrage durch und löscht alle gefundenen DNS-Records der konfigurierten Zone.

### Backup erstellen

Vor dem Ausführen empfiehlt sich ein Export der aktuellen DNS-Zone, z. B. über:

```bash
curl -s -X GET "https://api.cloudflare.com/client/v4/zones/${ZONE_ID}/dns_records_export" \
  -H "X-Auth-Email: ${EMAIL}" \
  -H "X-Auth-Key: ${KEY}" > backup.txt
```

So kannst du die Einträge im Bedarfsfall über den [Import-Dialog](https://developers.cloudflare.com/dns/manage-dns-records/how-to/import-and-export/) im Cloudflare-Dashboard wiederherstellen.

### Sicherheitshinweis

Der **Global API Key** gewährt vollen Zugriff auf deinen kompletten Cloudflare-Account.

### Lizenz

Dieses Projekt steht ohne explizite Lizenz zur freien Verfügung. Nutzung auf eigene Verantwortung.

### Autor & Kontakt

**Ioannis Toptsis (Janni)**

- 🌐 Website: [janni.fun](https://janni.fun)
- 💬 Discord: [janni.fun/discord](https://janni.fun/discord)

**[↑ Zur Sprachauswahl](#cloudflare-dns-clear)**

---

<a id="english"></a>
## 🇬🇧 English

A simple Bash script that fetches **all DNS records** of a Cloudflare zone via the Cloudflare API v4 and then deletes them completely.

> ⚠️ **Warning – destructive action!**
> This script deletes **every single** DNS record of the specified zone, without confirmation and without any automatic way to restore them. Only use it if you are absolutely certain you want to discard the entire DNS configuration of the zone. It is strongly recommended to export a backup of the zone beforehand (see [Creating a backup](#creating-a-backup)).

### How it works

1. The script fetches up to 500 DNS records of the zone via `GET /zones/{zone_id}/dns_records`.
2. `jq` is used to extract all record IDs from the response.
3. `DELETE /zones/{zone_id}/dns_records/{id}` is then called for each ID, removing the respective record.

### Requirements

- Bash (Linux, macOS, or WSL/Git Bash on Windows)
- [`curl`](https://curl.se/)
- [`jq`](https://stedolan.github.io/jq/) for parsing the JSON responses
- A Cloudflare account with:
  - Zone ID of the affected domain
  - Account email address
  - Global API Key (`X-Auth-Key`)

### Installation

```bash
git clone <repository-url>
cd shellscript_cloudflare-dns-clear
chmod +x script.sh
```

### Configuration

Open `script.sh` and enter the three required values at the top:

```bash
ZONE_ID=your_zone_id
EMAIL=your@email.com
KEY=your_global_api_key
```

| Variable  | Description                                                  | Where to find it in Cloudflare                             |
|-----------|-----------------------------------------------------------------|-----------------------------------------------------------|
| `ZONE_ID` | ID of the zone (domain) whose DNS records will be deleted       | Dashboard → Domain → Overview (right sidebar)               |
| `EMAIL`   | Email address of the Cloudflare account                          | Account settings                                            |
| `KEY`     | Global API Key                                                    | My Profile → API Tokens → Global API Key                    |

### Usage

```bash
./script.sh
```

The script runs without further confirmation and deletes all DNS records found in the configured zone.

### Creating a backup

Before running the script, it is recommended to export the current DNS zone, e.g. via:

```bash
curl -s -X GET "https://api.cloudflare.com/client/v4/zones/${ZONE_ID}/dns_records_export" \
  -H "X-Auth-Email: ${EMAIL}" \
  -H "X-Auth-Key: ${KEY}" > backup.txt
```

This lets you restore the records if needed via the [import dialog](https://developers.cloudflare.com/dns/manage-dns-records/how-to/import-and-export/) in the Cloudflare dashboard.

### Security notice

The **Global API Key** grants full access to your entire Cloudflare account.

### License

This project is provided without an explicit license, free to use. Use at your own risk.

### Author & Contact

**Ioannis Toptsis (Janni)**

- 🌐 Website: [janni.fun](https://janni.fun)
- 💬 Discord: [janni.fun/discord](https://janni.fun/discord)

**[↑ Back to language selection](#cloudflare-dns-clear)**
