# Pillion: bike pooling on Google Sheet + Apps Script + GitHub Pages

Koi server ya paisa nahi lagta.

- **Google Sheet** = database
- **Apps Script** (`apps-script/Code.gs`) = API (login, ride post, search, booking)
- **GitHub Pages** (`index.html`) = website, phone aur laptop dono par chalti hai

## Files

| File | Kaam |
| --- | --- |
| `apps-script/Code.gs` | Poora backend. Apps Script editor mein paste karna hai |
| `apps-script/appsscript.json` | Time zone (Asia/Kolkata) aur web app access settings |
| `index.html` | Website. GitHub par upload hogi |
| `config.js` | Isme sirf apna Apps Script URL paste karna hai |

## Setup (lagbhag 15 minute)

### 1. Google Sheet banao
sheets.google.com par naya blank Sheet banao, naam rakho `Pillion DB`. Isme kuch likhna nahi hai.

### 2. Backend code paste karo
1. Sheet mein **Extensions → Apps Script** kholo.
2. Left mein `Code.gs` ka purana code hata kar `apps-script/Code.gs` ka poora code paste karo.
3. Left mein **Project Settings** (gear icon) kholo aur **Show "appsscript.json" manifest file in editor** tick karo.
4. Editor mein wapas jao, `appsscript.json` kholo aur uska content `apps-script/appsscript.json` se replace karo.
5. **Save** karo (Ctrl+S).

### 3. `setup` chalao (sirf ek baar)
1. Upar function dropdown mein `setup` chuno aur **Run** dabao.
2. Google permission maangega. **Review permissions** → apna account → **Advanced → Go to project (unsafe) → Allow**. Ye apna hi script hai, isliye ye warning normal hai.
3. Sheet mein 5 tabs ban jayenge: Users, Rides, Bookings, Places, Settings. Places aur Settings mein starting data bhi aa jata hai.

### 4. Web app deploy karo
1. **Deploy → New deployment** → gear icon → **Web app**.
2. **Execute as: Me**, **Who has access: Anyone**.
3. **Deploy** dabao aur **Web app URL** copy karo (`https://script.google.com/macros/s/.../exec`).
4. Us URL ko browser mein kholo. Agar `{"ok":true,"data":{"name":"Pillion API","status":"running"}}` dikhe, to backend ready hai.

### 5. Website ko API se jodo
`config.js` kholo aur URL paste karo:

```js
window.PILLION_API = "https://script.google.com/macros/s/XXXXXXXX/exec";
```

### 6. GitHub Pages par daalo
1. github.com par naya repository banao (jaise `pillion`), public rakho.
2. **Add file → Upload files** se `index.html` aur `config.js` upload karo, aur commit karo.
3. **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save.**
4. Ek-do minute baad site `https://<username>.github.io/pillion/` par live hogi.

### 7. Test karo
1. Site kholo aur do alag mobile numbers se do account banao (ek laptop par, ek phone par).
2. Ek account se ride post karo, doosre se date chun kar search karo aur book karo.
3. Booking ke baad My trips mein rider ka number dikhega.

## Roz ke kaam (admin)

| Kaam | Kahan |
| --- | --- |
| Rider ko "KYC verified" badge dena | `Users` tab, us user ki `kyc` column mein `verified` likho |
| Kisi user ko block karna | `Users` tab, `status` column mein `blocked` likho |
| Naya shehar ya jagah jodna | `Places` tab mein nayi row: `id` (chhota code), `name`, `lat`, `lng` |
| Fare rate ya platform fee badalna | `Settings` tab: `rate` (₹ per km), `feePct` (%) |
| Bookings aur rides dekhna | `Bookings` aur `Rides` tabs |

Places aur Settings ka badlav 5 minute mein site par dikhta hai.

## Code badalne ke baad
`Code.gs` edit karo, phir **Deploy → Manage deployments → pencil icon → Version: New version → Deploy**. URL wahi rehta hai.

## Dhyan rakhne wali baatein
- **Sheet private rakho.** Ise kisi ke saath share mat karo. Script aapke naam se chalti hai aur sirf wahi Sheet padhti hai.
- **Mobile number verify nahi hota.** Login number + PIN se hota hai. PIN hashed save hota hai aur 5 galat try par account 15 minute ke liye lock ho jata hai. Pilot ke liye theek hai. Public launch se pehle SMS OTP jodna chahiye.
- **Number sirf booking ke baad dikhta hai**, rider aur passenger dono ko ek doosre ka.
- **Speed:** har request mein 1 se 2 second lag sakte hain. Ye Apps Script mein normal hai.
- **Limits:** free Apps Script aur Sheet 100 ke aas-paas users ke pilot ke liye kaafi hain. Isse upar jaane par Supabase ya Firebase par shift karna better hoga.
- **Legal aur insurance:** private bike se paise lene par Motor Vehicles Act ke rules aur pillion ke liye insurance cover check kar lo. App fare ko fuel cost ke aas-paas rakhta hai taaki ride cost sharing rahe.

## Aage kya jod sakte ho
Ride ke baad rating, booking par email (`MailApp`), SMS OTP login, ride ka reminder, aur Supabase par migration.
