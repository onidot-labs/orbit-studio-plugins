# orbit-studio-plugins

Orbit Studio 플러그인 마켓의 목록 저장소다. Orbit Studio의 설정 › 플러그인 › 마켓이 이 저장소 기본 브랜치의 `catalog.json`을 읽어 찾기·설치·업데이트를 한다.

- 목록에는 onidot 공식 플러그인만 싣는다. 다른 플러그인은 Studio에서 zip 파일이나 저장소 주소로 직접 설치한다.
- 플러그인 zip은 이 저장소(또는 항목의 `repo`)의 GitHub 릴리스에 올린다. Studio는 받은 zip의 SHA-256이 목록과 다르면 설치하지 않는다.

## catalog.json

```json
{"version": 1, "plugins": [
  {"id": "db", "name": "DB", "description": "…", "repo": "onidot-labs/orbit-studio-plugins",
   "version": "0.9.0", "studioApi": "^0.1.23", "sha256": "<zip의 SHA-256, 64자리 hex>", "tag": "db-v0.9.0"}
]}
```

| 칸 | 뜻 |
| --- | --- |
| `id`·`name`·`description` | 플러그인 `plugin.json`과 같은 값 |
| `repo` | 릴리스가 있는 저장소(`owner/name`) |
| `version` | 최신 판(semver). zip 안 `plugin.json`의 id·판과 같아야 설치된다 |
| `studioApi` | 필요한 플러그인 API 범위. Studio 판이 맞지 않으면 설치하지 않고 이유를 보인다 |
| `sha256` | 릴리스 zip의 SHA-256 |
| `tag` | 릴리스 태그. 없으면 `v<version>`. 한 저장소에 여러 플러그인을 올리므로 `<id>-v<version>`으로 쓴다 |

## 새 판 올리기

1. orbit-studio 저장소에서 플러그인 판(`plugin.json`·`package.json`)을 올리고 빌드한다(`npm run build`).
2. `node scripts/pack-plugin.mjs plugins/<id> --tag <id>-v<version>`으로 zip과 SHA-256, 목록 항목을 만든다.
3. 이 저장소에 릴리스 `<id>-v<version>`을 만들어 zip 하나를 올린다.
4. `catalog.json`의 그 항목을 바꿔 PR로 병합한다.
