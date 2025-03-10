

### **Google Colab-Compatible Slack Profile Picture Checker**
# 1. Open [Google Colab](https://colab.research.google.com/).
# 2. Enter your **Slack API Token** when prompted.
# 2.1 - https://api.slack.com/changelog/2020-02-legacy-test-token-creation-to-retire
# 2.2 - create a new app and add scopes related to user and read only
# 2.3 - Go to your app. Under Oath subsection copy "Bot User OAuth Token" and enter into when prompted in the script
# 3. Run the code to get a list of users who have not updated their profile pictures.


import requests
import pandas as pd
from google.colab import files

# Step 1: Enter your Slack API Token securely
SLACK_API_TOKEN = input("Enter your Slack API Token: ").strip()

def get_users_without_profile_pics():
    url = "https://slack.com/api/users.list"
    headers = {"Authorization": f"Bearer {SLACK_API_TOKEN}"}

    response = requests.get(url, headers=headers)
    users_without_pics = []

    if response.status_code == 200:
        data = response.json()
        if data.get("ok"):
            for user in data["members"]:
                # Ignore bots and deactivated users
                if user.get("is_bot") or user.get("deleted"):
                    continue

                profile = user.get("profile", {})
                image_original = profile.get("image_original", "")

                # Check if the profile picture is default
                if not image_original or "slack.com" in image_original and "avatar" in image_original:
                    users_without_pics.append({
                        "ID": user["id"],
                        "Name": user["real_name"],
                        "Username": user["name"]
                    })
        else:
            print("Error:", data.get("error"))
    else:
        print("Failed to fetch users:", response.status_code)

    return users_without_pics

# Step 2: Get users who haven't updated profile pictures
users_without_pics = get_users_without_profile_pics()

# Step 3: Display the result in a DataFrame
df = pd.DataFrame(users_without_pics)
if not df.empty:
    print(f"\nFound {len(df)} users without profile pictures:\n")
    display(df)  # Display the table in Colab

    # Step 4: Save as CSV and offer download
    csv_filename = "slack_users_without_profile_pics.csv"
    df.to_csv(csv_filename, index=False)
    print("\nCSV file saved. Click the link below to download:")
    files.download(csv_filename)
else:
    print("\nAll users have profile pictures! 🎉")
