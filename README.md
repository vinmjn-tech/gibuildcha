# gibuildcha
Genshin Build Character API

<div align="center">

# API 123giguide

</div>

---

## Fitur

-  **Cepat** “ Mempermudah build aplikasi/website guide genshin Anda
-  **Mudah** “ Plug and play
-  **Tanpa Plugin** “ Bisa di kode apa saja, baik npm atau node.js
-  **Integrasi** “ Dapat terhubung di JavaScript Vanilla -  **Keluasan** “ Mempermudah kode Mobile Developer masuk (Seperti Swift, Kotlin, dan Java)

---


## Kode

| Variabel       | Deskripsi                   | Contoh                         |
| :------------- | :-------------------------- | :----------------------------- |
| `index`         | Index                 | `https://vinmjn-tech.github.io/gibuildcha/characters-index.json`                         |
| `mondstadt` | Mondstadt           | `https://vinmjn-tech.github.io/gibuildcha/characters-mondstadt.json` |
| `liyue`     | Liyue                 | `https://vinmjn-tech.github.io/gibuildcha/characters-liyue.json`                  |
| `inazuma`     | Inazuma                | `https://vinmjn-tech.github.io/gibuildcha/characters-inazuma.json`                  |
| `sumeru`     | Sumeru                 | `https://vinmjn-tech.github.io/gibuildcha/characters-sumeru.json`                  |
| `fontaine`     | Fontaine                | `https://vinmjn-tech.github.io/gibuildcha/characters-fontaine.json`                  |
| `natlan`     | Natlan                | `https://vinmjn-tech.github.io/gibuildcha/characters-natlan.json`                  |
| `nodkrai`   | Nod-Krai          | `https://vinmjn-tech.github.io/gibuildcha/characters-nodkrai.json`     |
| `snezhnaya`     | Snezhnaya                 | `https://vinmjn-tech.github.io/gibuildcha/characters-snezhnaya.json`                  |

---

## Testing API

```
https://vinmjn-tech.github.io/gibuildcha/characters-nama_region.json
```

---

## Cara Menggunakan

- [Swift]
```
import Foundation

struct Region: Codable {
    let id: String
    let name: String
    let file: String
}

struct RegionIndex: Codable {
    let regions: [Region]
}

struct CharacterApiClient {
    private let baseURL = "https://vinmjn-tech.github.io/gibuildcha"

    func fetchCharactersJSON(forRegionID regionID: String) async throws -> Data {
        let urlString = "\(baseURL)/characters-\(regionID).json"
        guard let url = URL(string: urlString) else {
            throw URLError(.badURL)
        }

        let (data, response) = try await URLSession.shared.data(from: url)
        guard let http = response as? HTTPURLResponse, (200...299).contains(http.statusCode) else {
            throw URLError(.badServerResponse)
        }
        return data
    }
}

// Contoh pemakaian:
Task {
    let client = CharacterApiClient()
    do {
        let jsonData = try await client.fetchCharactersJSON(forRegionID: "mondstadt")
        print(String(data: jsonData, encoding: .utf8) ?? "")
    } catch {
        print("Error: \(error)")
    }
}
```

- [Kotlin]
```
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.withContext
import java.net.HttpURLConnection
import java.net.URL

class CharacterApiClient(
    private val baseUrl: String = "https://vinmjn-tech.github.io/gibuildcha"
) {
    suspend fun fetchCharactersJson(regionId: String): String = withContext(Dispatchers.IO) {
        val endpoint = "$baseUrl/characters-$regionId.json"
        val connection = (URL(endpoint).openConnection() as HttpURLConnection).apply {
            requestMethod = "GET"
            connectTimeout = 10_000
            readTimeout = 10_000
        }

        try {
            if (connection.responseCode !in 200..299) {
                throw Exception("HTTP Error: ${connection.responseCode}")
            }
            connection.inputStream.bufferedReader().use { it.readText() }
        } finally {
            connection.disconnect()
        }
    }
}

// Contoh pemakaian di coroutine:
// val json = CharacterApiClient().fetchCharactersJson("liyue")
// println(json)
```

- [Java]
```
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

public class CharacterApiClient {
    private static final String BASE_URL = "https://vinmjn-tech.github.io/gibuildcha";

    public String fetchCharactersJson(String regionId) throws Exception {
        String endpoint = BASE_URL + "/characters-" + regionId + ".json";
        URL url = new URL(endpoint);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        conn.setConnectTimeout(10000);
        conn.setReadTimeout(10000);

        int status = conn.getResponseCode();
        if (status < 200 || status >= 300) {
            throw new RuntimeException("HTTP Error: " + status);
        }

        StringBuilder sb = new StringBuilder();
        try (BufferedReader reader = new BufferedReader(
                new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
            String line;
            while ((line = reader.readLine()) != null) {
                sb.append(line).append('\n');
            }
        } finally {
            conn.disconnect();
        }
        return sb.toString();
    }

    public static void main(String[] args) throws Exception {
        CharacterApiClient client = new CharacterApiClient();
        System.out.println(client.fetchCharactersJson("inazuma"));
    }
}
```

- [Python]
```
import json
import urllib.request

BASE_URL = "https://vinmjn-tech.github.io/gibuildcha"

REGIONS = [
    {"id": "limited",   "name": "Limited",                      "file": "characters-limited.json"},
    {"id": "mondstadt", "name": "Mondstadt",                    "file": "characters-mondstadt.json"},
    {"id": "liyue",     "name": "Liyue",                        "file": "characters-liyue.json"},
    {"id": "inazuma",   "name": "Inazuma",                      "file": "characters-inazuma.json"},
    {"id": "sumeru",    "name": "Sumeru",                       "file": "characters-sumeru.json"},
    {"id": "fontaine",  "name": "Fontaine",                     "file": "characters-fontaine.json"},
    {"id": "natlan",    "name": "Natlan",                       "file": "characters-natlan.json"},
    {"id": "nodkrai",   "name": "Nod Krai",                     "file": "characters-nodkrai.json"},
    {"id": "snezhnaya", "name": "Snezhnaya",                    "file": "characters-snezhnaya.json"},
    {"id": "khaenriah", "name": "Khaenri'ah / Belum_Diketahui", "file": "characters-khaenriah.json"},
]

def get_characters(region_id: str):
    url = f"{BASE_URL}/characters-{region_id}.json"
    with urllib.request.urlopen(url, timeout=10) as resp:
        return json.loads(resp.read().decode("utf-8"))

if __name__ == "__main__":
    data = get_characters("fontaine")
    print(json.dumps(data, indent=2, ensure_ascii=False))
```

---

## Lisensi

Didistribusikan di bawah lisensi **MIT**. Lihat [LICENSE](LICENSE) untuk detail.

---

<div align="center">

Dibuat dengan oleh: [123giguide](https://github.com/vinmjn-tech/gibuildcha)

Tim Grup WhatsApp Kami
- Vin: Guide Genshin Impact Community
- Yuyu: Informasi Genshin Di Grup
- Cha: Intro Grup
- Justryan: Vibe Mobile Developer With AI Help

**Kasih bintang kalau project ini membantu!**

</div>