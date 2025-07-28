# my_post

pip install gspread google-auth


from google.oauth2 import service_account
from google.cloud import storage
from google.auth.transport.requests import Request

SERVICE_ACCOUNT_FILE = r'smart-home-467303-51cc9f0c9990.json'
PROJECT_ID = 'smart-home-467303'

# สร้าง scope ที่ต้องใช้สำหรับ Google Cloud Storage
SCOPES = ['https://www.googleapis.com/auth/cloud-platform']

try:
    print("กำลังโหลด Credential จาก:", SERVICE_ACCOUNT_FILE)
    credentials = service_account.Credentials.from_service_account_file(
        SERVICE_ACCOUNT_FILE,
        scopes=SCOPES
    )

    # รีเฟรช token ให้เรียบร้อยก่อนใช้
    credentials.refresh(Request())
    print("โหลด Credential สำเร็จ")

    client = storage.Client(credentials=credentials, project=PROJECT_ID)

    print("กำลังดึงรายการ bucket...")
    buckets = list(client.list_buckets())
    for bucket in buckets:
        print(bucket.name)

except Exception as e:
    print("ผิดพลาด:", e)
