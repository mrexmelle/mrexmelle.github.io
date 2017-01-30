# django-movies-data

Contoh ini akan mendemonstrasikan cara untuk membuat sebuah aplikasi bot dengan **Django framework** dan terintegrasi dengan **LINE Messaging API** dan **LINE Bot SDK** untuk memerlihaktkan kegunaan dari fitur LINE Messaging API. Data film diambil dari [OMDb API](https://www.omdbapi.com/). Aplikasi ini dijalankan di **Heroku**.

## Langkah Untuk Membuat
* Pertama, anda harus membuat akun LINE@, mengaktifkan Messaging API, dan memasukkan URL dari Webhook anda dengan [cara ini](Integration.html).

* Lalu buat sebuah *project* dan aplikasi dengan framework django dengan cara

* Selanjutnya, tambah file  `app_properties.py` di direktori *<nama_app_anda>*, dan isi dengan channel secret dan channel access token anda, seperti berikut:

	```ini
channel_secret = <channel_secret_anda>
channel_access_token = '<channel_access_token_anda>'
	```
* Pada saat anda menerima sebuah event, maka anda akan mendapatkan data JSON:

	```json
	{
		"events":
		[
			{
				"type":"message",
				"replyToken":"610147d637dc4ef091cb47ca45289ba6",
				"source":
				{
					"userId":<target's_user_id>,
					"type":"user"
				},
				"timestamp":1484622624865,
				"message":
				{
					"type":"text",
					"id":"5513240775730",
					"text":"title \"Iron Man\""
				}
			}
		]
	}
	```
	Dari data ini anda bisa mendapatkan tipe dari event, replyToken, siapa yang melakukan event tersebut, dan detail lainnya dari event.

* Untuk membalas sebuah event pesan dari pengguna, anda dapat menggunakan **Reply Message API**. Pada contoh ini, digunakan Java SDK.

	```python
	line_bot_api = LineBotApi(channel_access_token)
    try:
        line_bot_api.reply_message(reply_token, TextSendMessage(text=text_message))
    except LineBotApiError as e:
        print('Exception is raised')
	```
	Dari source code diatas, terkomposisi data JSON dibawah. Anda hanya memerlukan **replyToken** untuk membalas pesan, tidak perlu menggunakan id dari target.
	
	```json
	{
		"replyToken":"<replyToken>",
		"messages":
		[
			{
				"type":"text",
				"text":"Plot: After being held captive in an Afghan cave, billionaire engineer Tony Stark creates a unique weaponized suit of armor to fight evil."
			}
		]
	}
	```

* Untuk mengirim pesan ke pengguna tertentu, anda dapat menggunakan **Push Message API**. Pada contoh ini, digunakan Java SDK.

	```python
	line_bot_api = LineBotApi(channel_access_token)
    try:
        line_bot_api.push_message(target_id, ImageSendMessage(original_content_url=poster_url,
                                                              preview_image_url=poster_url))
    except LineBotApiError as e:
        print('Exception is raised')
	```
	Dari source code diatas, terkomposisi data JSON dibawah. Untuk mengirimkan pesan ini, anda membutuhkan **id** dari target pengiriman.
	
	```json
	{
		"to":"<target's_id>",
		"messages":
		[
			{
				"type":"text",
				"text":"plot \"iron man\" - user\uDBC0\uDC8D"
			}
		]
	}
	```

* Pada Messaging API terdapat sebuah jenis pesan yang disebut **Template Message**. Pada contoh ini, digunakan salah satu template message yaitu **Carousel Template**. Cara membuat carousel template message:

	```python
	carousel_template = CarouselTemplate(columns=[
                                CarouselColumn(thumbnail_image_url=poster_url, title=title, text='Select one for more info', actions=[
                                        MessageTemplateAction(label='Full Data', text='Title \"'+title+'\"'),
                                        MessageTemplateAction(label='Summary', text='Plot \"'+title+'\"'),
                                        MessageTemplateAction(label='Poster', text='Poster \"'+title+'\"')
                                        ]),
                                CarouselColumn(thumbnail_image_url=poster_url, title=title, text='Select one for more info', actions=[
                                        MessageTemplateAction(label='Released Date', text='Released \"'+title+'\"'),
                                        MessageTemplateAction(label='Actors', text='Actors \"'+title+'\"'),
                                        MessageTemplateAction(label='Awards', text='Awards \"'+title+'\"')
                                        ])
                                ])
    template_message = TemplateSendMessage(alt_text='Your search result', template=carousel_template)
    ```
	Dari source code diatas, terkomposisi data JSON dibawah. Untuk mengirimkan pesan ini, digunakan metode pengiriman dengan **Push Message API** maka dari itu, anda membutuhkan **id** dari target pengiriman.
	
	```json
	{
		"to":"<target's_id>",
		"messages":
		[
			{
				"type":"template",
				"altText":"Your search result",
				"template":
				{
					"type":"carousel",
					"columns":
					[
						{
							"thumbnailImageUrl":"https://images-na.ssl-images-amazon.com/images/M/MV5BMTczNTI2ODUwOF5BMl5BanBnXkFtZTcwMTU0NTIzMw@@._V1_SX300.jpg",
							"title":"Iron Man",
							"text":"Select one for more info",
							"actions":
							[
								{
									"type":"message",
									"label":"Full Data",
									"text":"Title \"Iron Man\""
								},
								{
									"type":"message",
									"label":"Summary",
									"text":"Plot \"Iron Man\""
								},
								{
									"type":"message",
									"label":"Poster",
									"text":"Poster \"Iron Man\""
								}
							]
						},
						{
							"thumbnailImageUrl":"https://images-na.ssl-images-amazon.com/images/M/MV5BMTczNTI2ODUwOF5BMl5BanBnXkFtZTcwMTU0NTIzMw@@._V1_SX300.jpg",
							"title":"Iron Man",
							"text":"Select one for more info",
							"actions":
							[
								{
									"type":"message",
									"label":"Released Date",
									"text":"Released \"Iron Man\""
								},
								{
									"type":"message",
									"label":"Actors",
									"text":"Actors \"Iron Man\""
								},
								{
									"type":"message",
									"label":"Awards",
									"text":"Awards \"Iron Man\""
								}
							]
						}
					]
				}
			}
		]
	}

	```

* Salah satu fitur dari Messaging API adalah anda dapat meninggalkan group atau room dengan menggunakan **Leave API**. Pada contoh di bawah, digunakan Java SDK.

	```python
	line_bot_api = LineBotApi(channel_access_token)
    try:
        if source_type == 'room':
            line_bot_api.leave_room(target_id)
        elif source_type == 'group':
            line_bot_api.leave_group(target_id)
    except LineBotApiError as e:
        print('Exception is raised')
	```
	Leave API hanya akan mengirimkan JSON kosong saat berhasil.

* Setelah selesai membuat aplikasi anda, anda dapat menjalankan aplikasi anda dengan [cara ini](heroku-overview.html).

## Penggunaan
Untuk menggunakan bot ini, keyword yang dibutuhkan adalah:

* title		: Memberikan data lengkap tentang film
* plot		: Memberikan ringkasan film
* poster	: Memberikan poster film
* released	: Memberikan tanggal film ditayangkan
* director	: Memberikan nama sutradara film
* writer	: Memberikan nama penulis skrip film
* actors	: Memberikan daftar pemeran utama di film
* awards	: Memberikan daftar penghargaan yang dimenangkan film
* carousel	: Memberikan pilihan kepada pengguna untuk data yang dapat diakses

Ketik ***keyword* + "*judul*"** pada bots untuk menggunakan keyword ini

## Tautan ke git repository

[django-movies-data](https://github.com/mrexmelle/django-movies-data)
