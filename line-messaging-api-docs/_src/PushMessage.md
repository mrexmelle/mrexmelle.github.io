#Push message

![pushrep](https://scdn.line-apps.com/n/_5/partner-center/img/lp/us/msgapi-figure4.png?v=1)

Mengirim pesan ke pengguna, grup, atau room kapan pun.

**INFO** Penggunaan Push Message API dibatasi untuk paket tertentu.

####HTTP request
`POST https://api.line.me/v2/bot/message/push`

####Request Headers
Request header | Deskripsi
---|---
Content-Type | "application/json"
Authorization | "Bearer "{Channel Access Token}

####Request Body
Berisi array dari objek "send message" sebagai pesan anda.

Label | Tipe | Deskripsi
---|---|---
to | String | ID penerima
messages | Array dari objek "send message" | Messages, Max: 5
**INFO** Gunakan ID yang dikembalikan via event webhook pada userId, groupId, atau roomId sebagai ID penerima. Jangan menggunakan LINE ID yang didapatkan dari LINE app.

####Response
Mengembalikan kode status **200 OK** dan objek JSON kosong.