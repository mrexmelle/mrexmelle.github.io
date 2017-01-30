#Reply message

![pushrep](https://scdn.line-apps.com/n/_5/partner-center/img/lp/us/msgapi-figure4.png?v=1)

Menanggapi event dari pengguna, grup, dan room.

Webhook digunakan untuk memberitahu anda saat sebuah event terjadi. Untuk event yang dapat anda tanggapi, sebuah **replyToken** diberikan untuk membalas pesan.

Karena replyToken dapat invalid dalam periode waktu tertentu, respon sebaiknya dikirimkan secepat mungkin setalah pesan diterima. **Reply token hanya dapat digunakan satu kali**.

####HTTP request
`POST https://api.line.me/v2/bot/message/reply`

####Request headers
Request header | Description
----|----
Content-Type	| "application/json"
Authorization |	"Bearer "{Channel Access Token}

####Request body
Berisi array dari objek "send message" sebagai pesan anda.

Field	| Type | Description
---|---|---
replyToken | String | replyToken diterima via webhook
messages | Array dari objek "send message" | Messages, Max: 5

####Respon
Mengembalikan kode status **200 OK** dan sebuah objek JSON kosong.

