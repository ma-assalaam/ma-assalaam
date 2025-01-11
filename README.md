- 👋 Hi, I’m @ma-assalaam
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
ma-assalaam/ma-assalaam is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
from google.oauth2 import service_account
from googleapiclient.discovery import build

# Autentikasi menggunakan Google Service Account
SCOPES = ['https://www.googleapis.com/auth/business.manage']
SERVICE_ACCOUNT_FILE = 'path_to_your_service_account_json_file.json'

credentials = service_account.Credentials.from_service_account_file(
    SERVICE_ACCOUNT_FILE, scopes=SCOPES)

# Membuat service API client
service = build('mybusinessbusinessinformation', 'v1', credentials=credentials)

# Mendapatkan Ulasan
def get_reviews(account_id, location_id):
    reviews = service.accounts().locations().reviews().list(
        parent=f'accounts/{account_id}/locations/{location_id}'
    ).execute()
    
    return reviews['reviews']

# Menanggapi Ulasan
def reply_to_review(account_id, location_id, review_id, response):
    service.accounts().locations().reviews().updateReply(
        name=f'accounts/{account_id}/locations/{location_id}/reviews/{review_id}',
        body={'comment': response}
    ).execute()

# Contoh Penggunaan
account_id = 'your_account_id'
location_id = 'your_location_id'
reviews = get_reviews(account_id, location_id)

# Menanggapi ulasan pertama
if reviews:
    first_review_id = reviews[0]['reviewId']
    reply_to_review(account_id, location_id, first_review_id, 'Terima kasih atas ulasan positif Anda!')
