# Íbúðaverð á Íslandi vs Bitcoin (ISK)

Mælaborð í Streamlit sem ber saman **íslenska íbúðaverðsvísitölu** og **Bitcoin verð í ISK** frá upphafi Bitcoin.

> 🆕 Uppfærsla: CoinGecko beiðnir eru nú **takmarkaðar við 1× á klukkustund** með skyndiminni (cache) til að forðast tímabundnar villur (t.d. 429/5xx).

---

## Skref‑fyrir‑skref leiðbeiningar (fyrir byrjendur)

### 1) Sækja skrárnar
- Afritaðu þessa möppu í tölvuna þína (eða notaðu “Download ZIP” úr samtalinu ef í boði).

### 2) Opna skipanalínu (Terminal)
- **Windows**: Opnaðu *Command Prompt* eða *PowerShell*.
- **Mac/Linux**: Opnaðu *Terminal*.

Farðu inn í möppuna þar sem skrárnar eru (t.d. `cd Downloads/iceland-housing-vs-btc-v2`).

### 3) Setja upp Python umhverfi
```bash
python -m venv .venv
# Virkja umhverfið
# Windows:
.venv\Scripts\activate
# Mac/Linux:
source .venv/bin/activate

pip install -r requirements.txt
```

### 4) Ræsa mælaborðið
```bash
streamlit run app.py
```
Vafrinn opnast sjálfur á **http://localhost:8501** (eða afritaðu slóðina í vafra).

### 5) Nota mælaborðið
- Í hliðarstikunni er **PXWeb API slóð** – prófaðu sjálfgefnu fyrst.
- Ef hún virkar ekki (breytingar hjá Hagstofu), farðu á <https://px.hagstofa.is/>, finndu íbúðaverðsvísitölu og smelltu á **API** til að fá rétta **JSON API** slóð. Límd þú slóðina í hliðarstikuna.
- Þú getur líka valið **Hlaða upp CSV** ef þú vilt koma með gögnin sjálfur (dálkar: `date`, `hpi`).

### 6) Um „rauntíma“ BTC verð og skyndiminni
- Appið sækir **BTC söguna** og **lifandi BTC→ISK** frá CoinGecko.\
- Til að **forðast tímabundnar villur**, eru bæði þessi köll **skyndiminni-læst í 1 klst**.\
  - Það þýðir: þó þú endurhleður síðuna oft, mun appið **ekki** hringja strax í CoinGecko; það notar síðustu niðurstöðu í **allt að 1 klst**.\
  - Eftir u.þ.b. klukkustund reynir það aftur sjálfkrafa.\
- Ef CoinGecko er niðri í augnablik: þú gætir séð skilaboðin *„ekki tiltækt í bili“* fyrir live-verð. Það lagast sjálfkrafa þegar næsta endurhleðsla á sér stað.

### 7) Dreifing (valkvætt)
- **Streamlit Community Cloud**: ýttu kóðanum í GitHub repo og tengdu við Streamlit Cloud.\
- **Docker** (fyrir netþjón):\n
```bash
docker build -t iceland-housing-vs-btc .
docker run -p 8501:8501 iceland-housing-vs-btc
```

### 8) Hvar breyti ég tíðni?
- Í `app.py` eru línur merktar **@st.cache_data(ttl=3600)**.\
- **`3600` sek. = 1 klst**. Viltu sjaldnar/oftar? Breyttu tölunni (t.d. `ttl=1800` fyrir 30 mín).

---

## Hvað er undir húddinu?
- Hagstofan (PXWeb) skilar **mánaðarlegri** vísitölu. CoinGecko skilar **daglegu/rauntíma** BTC.\
- Appið breytir BTC í **mánaðarlega samantekt** (meðaltal eða lokagildi) svo samanburðurinn verði sanngjarn.
- Svo teiknum við 4 myndrit: Íbúðavísitölu, BTC (með valkvæðu log-skafti), hlutfallið `Íbúðavísitala / BTC` og normalíseraðar línur (=100 við upphaf).

## Kröfur
Sjá `requirements.txt` (Streamlit, pandas, numpy, requests, plotly).

## Takmarkanir og ábendingar
- PXWeb breytist stundum; ef API slóð hættir að virka, afritaðu nýja slóð af vefnum og límdu í appið.
- CoinGecko getur stundum skilað 429/5xx; 1 klst skyndiminni og mildur retry minnka líkur á vandræðum.
