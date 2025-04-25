import requests
import time

cookies = {
    'USER_LOCALE': 'en',
    '_gcl_au': '1.1.1477323571.1741898721',
    '_ga': 'GA1.1.2014086454.1741898721',
    '_rdt_uuid': '1741898728140.e547c65f-4c8a-48ac-bf47-8030b9849bca',
    '__cf_bm': 'Z6XJ8zon6VQUQVYYS6kEwij.tk0a_xSQxd.WA4_RJGY-1742221237-1.0.1.1-dnTTd3oNJcSIM5E1Cbr0hVbJA46P47uCp4qOxwekLtkWRXHZZRZF0kye1qhUZ8RStdmS0.ONVT5Kxm7Lcw3DDX_NKmZdTE3XIeZGctNL5MU',
    'cf_clearance': 'pOSoSIdLxn3IdB7IdMSguhh6d2gnAMiapYyUBKiXCIA-1742221241-1.2.1.1-Y2gaZxphRcuS4vaTctK56vtsKxA.8IlIlOc9_uef5wP6UcW_Q4FrGhzGHAA.8Bwo0tGmh1RccKYDMq_xzZvyfaJVEKcxfivEtlmQWe15MR6z6VoCRWSsOGVrZVPwrdV0bsE9uoI2wxXS6l1bQup9O0Vc4wAGe0MUh8brenPW7ndUUtpDKFXeXcw0vmk_cyZ8vdcBR9.zdZQhfEeFQeXERriiulxtIz4hp.qj70lgFSlbMx1t1FyNN44SJugGmO8Sbm8rnx5lsdZzebpc.e1N6OmW5eLwXfLHEYmcbjT.XKg.cDqaPL3Prq9a_TlRc.jruwFJR6p_QEVpfrP1HVWz5HlY5Jy6M.aN09EwowUGIWs',
    'volume': '100',
    '__stripe_mid': 'ca458a80-c2d6-426d-b8c5-28aea0fd78b82cb3a3',
    '__stripe_sid': '2b7d5e66-56ec-4145-846d-ff9c735c290e19a3b4',
    '_dd_s': 'logs=1&id=cbf15abb-9114-44ea-9d08-b2973d84ac66&created=1742221241658&expire=1742222274706&rum=0&lock=cb7bbc75-ca7c-4dbd-a748-a8e4f552b393',
    'XSRF-TOKEN': 'eyJpdiI6IlNWWjYrTDZJM21iQ0tlckV6ODZ2dmc9PSIsInZhbHVlIjoiVkJkcCs4VzFqZEk4Z0NNbWhvNlByd3JVYjM4eTVVSWk2Rzl4L2VJOVppUWszdHB6WDJRVWczbEFRYnFVZHB0bkZTa051WEdUZUhmVEZNK3plSUVZNndwdVhhZHluREtmZW84Yjl0a3czU0dRb1oyM2xsbkZ0cS94M3NidWtadnIiLCJtYWMiOiJhZGExYjc2NDNiYTEyYTA5NjBjZWM2MmI3N2RlYmYzZmZlYmQzNjg5MTY2OTNlYTRjNmYyNzYyMjBlNWRiYTdhIiwidGFnIjoiIn0%3D',
    'kick_session': 'eyJpdiI6Im91WmV4ZlIwUy9pMzdiYk1sS3JSZUE9PSIsInZhbHVlIjoiL0RaN3VHSzI5b1dRRmtCWndsNXRNZGtkeWlVT211ZE51b1VVa1A3M2EvT0xaRW9yeGNnK2ZCYjFZcitkaTFkYVp2aU9saisxQ2I5Nno5dlNMYzltdFh4U0kraEtqYjB5Wm1lOVpmT3JTc2VpOExOZytxN1o2NU03QnBJQzV4TlUiLCJtYWMiOiI1MjEyYzJhODAyODRkZmJhYTdlYzA4OTZjNDc5NmExNjUxNGQxMzFmOGYyOWY3Mzg1ZjJkNjY0MGIzMTg3ZDJlIiwidGFnIjoiIn0%3D',
    'QNBJYEwTrX7QBCqz8wUK3ZX4wRHc6lJ2qJEu5VgN': 'eyJpdiI6IkZjK2lpSnN0dFR2aFk0ZXZuZ1hrTmc9PSIsInZhbHVlIjoiWHRXNjRmL0JxcUdzV0VhM01yWUVEL1cyYjIzcWFZRmM5YkVpbmdwZzRUWVhTcEtJTlZCczZVWVBKWUUzYjFmVkZXNmZLaWR3Ti96RitESmZDdDNqZUF4a2pYbm4yNmFzejhFckZJYXBUdDMzSDJLUEpzbENCN2ZJNGV1UUhkSTJyaFVtVDFXNjhucFZkQ2NpQ1lnY1MvelhaQUN5NHpIdTI1Mkx1endJVGdwbmREU3NHZU9JNDQ0Sm5nMGRrekNFTTNpK0M4cEFMWWhxREpyZHJlSy9kWjh4NEh5WDh4K0tYTG05MWcrN0xlYm42c1NpSjFFY2twRW1hVk1oQWg5LzhhSE56dExMbnlaVkJISHFVbmMyanhqOFp0R0hpU1NGbGJMSjhXQlZ3SWtWOGdMVHZac0R4cnR0NFJiazRyVTY4NlJaNjJtQ1J1NUdya3owdHc4Y1RVRHBDb3NzS3ExOG5QY3MxL2d0SHJleFNQdERnUlc0Z2VkRGFQbW9HamR3VlRXMEpDZGsrVkJPY21CRzhiR1hybDNOampkVVl5allzQXl5ckR4MEdMTT0iLCJtYWMiOiI0MTA0ZTU5NDJmYzk0NjU3ZmNkZDVhNGExMzYzYTQwZmIzOTc0NGNlN2I3MjVmMTI0MDFlM2YxMzc5ZDgzMDUwIiwidGFnIjoiIn0%3D',
    'session_token': '200019874%7Ci8WYkf3xsAAPdY5biYnik0UsTbJAkgHcw9GcW9Ky',
    'KP_UIDz-ssn': '0bLGoMuzh89gQxAIHTprp6l0Pu8qylKlRgVLR3bXcy8gfAt28TQ5IJ9KxoFuYOUVOIPxGbv0LXNmKmQ4SrvOGs1Zw5e6LdxtMFQakf6aK7Zly6hQGJbXu5GUAkhTOBDsHxLu8nhV7ReveyYEgtedHv81f8eUBSxfljGMhoRYvw8',
    'KP_UIDz': '0bLGoMuzh89gQxAIHTprp6l0Pu8qylKlRgVLR3bXcy8gfAt28TQ5IJ9KxoFuYOUVOIPxGbv0LXNmKmQ4SrvOGs1Zw5e6LdxtMFQakf6aK7Zly6hQGJbXu5GUAkhTOBDsHxLu8nhV7ReveyYEgtedHv81f8eUBSxfljGMhoRYvw8',
    '_cfuvid': 's0_P.4jGsKjRW6lvFa6NoxwCgkh1P7qWC6tY0oJVTi8-1742221642919-0.0.1.1-604800000',
    '_ga_YS2DLSGQ53': 'GS1.1.1742221242.3.1.1742221689.39.0.470878563',
}

headers = {
    'authority': 'kick.com',
    'accept': 'application/json',
    'authorization': 'Bearer 200019874|i8WYkf3xsAAPdY5biYnik0UsTbJAkgHcw9GcW9Ky',
    'content-type': 'application/json',
    'origin': 'https://kick.com',
    'referer': 'https://kick.com/raydin',
    'user-agent': 'Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Mobile Safari/537.36',
}

json_data = {
    'content': '🖤💚❤️',
    'type': 'message',
}

while True:
    try:
        response = requests.post(
            'https://kick.com/api/v2/messages/send/39315024',
            cookies=cookies,
            headers=headers,
            json=json_data
        )
        print(f"Status Code: {response.status_code}, Response: {response.text}")
    except Exception as e:
        print(f"An error occurred: {e}")
    
    time.sleep(3)  # الانتظار لمدة 4 ثواني بين كل طلب
