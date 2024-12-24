```
This file contains instructions for updating usernames and passwords across these commonly used tools.
Ensure you have the necessary admin permissions to perform these changes.
```

# Steps to Change a Username in SonarQube UI

## 1. Log in to SonarQube
- Open the SonarQube web interface, typically available at:
  ```
  http://<your-sonarqube-server>:9000
  ```
- Log in with an administrator account.

## 2. Navigate to User Management
- Go to the top-right menu and click on **Administration**.
- In the left-hand menu, select **Security > Users**.

## 3. Find the User
- In the **Users** list, locate the user whose username you want to change.
- Use the search bar if necessary.

## 4. Edit the User Details
- Click the **Edit** (pencil) icon or the user’s name.
- Update the username field to the desired new name.
- Save the changes.

## 5. Assign Groups to the User
- Navigate to the user’s details page.
- Add the user to the following groups to assign the appropriate permissions:
  - **sonar-administrators**: Grants admin privileges.
  - **sonar-users**: Grants basic user access.
- Save the changes.

## 6. Log Out and Log In
- If the username change affects the current session, the user may need to log out and log in using the new username.

---

## Notes:
- Ensure the new username is unique and complies with your organization's naming policies.
- If the UI does not support username changes, you may need to:
  - Create a new user with the desired username and reassign permissions.
  - Or, directly update the SonarQube database (advanced users only).


# Steps to Change a Username and Password in Grafana Using the Web UI

## 1. Log in to Grafana
- Open the Grafana web interface, typically available at:
  ```
  http://<your-grafana-server>:3000
  ```
- Log in with an administrator account.

## 2. Navigate to User Management
- From the left-hand menu, click on **Configuration**.
- Select **Users** from the dropdown menu.

## 3. Locate the User
- Search for the user whose username or password you want to change.
- Click on the user’s name to open their profile.

## 4. Edit the Username
- In the user’s profile, locate the **Username** field.
- Update the username to the desired new name.
- Save the changes.

## 5. Change the Password
- In the same profile, locate the **Password** field.
- Enter and confirm the new password.
- Save the changes.

## 6. Log Out and Log In
- The user may need to log out and log in with the updated username and password to reflect the changes.

---

## Notes:
- Ensure the new username is unique and complies with your organization's naming conventions.
- Use a strong, secure password that meets your organization’s security policies.
- If the web UI does not allow direct username or password changes, you may need to create a new user with the desired credentials and reassign roles and permissions.
- Always back up your Grafana configuration or database before making significant changes.



# Method 2: Using the Jenkins Web UI (if admin access is available)

## 1. Log in to Jenkins
- Open the Jenkins web interface, typically available at:
  ```
  http://<your-jenkins-server>:8080
  ```
- Log in using an administrator account.

## 2. Navigate to Manage Users
- From the main dashboard, click on **Manage Jenkins** in the left-hand menu.
- Under the **Security** section, click on **Manage Users**.

## 3. Find the Admin User
- Locate the user whose username or password you want to change (e.g., `admin`).
- Click on the user’s name to view their profile.

## 4. Create a New User (Optional Step)
- If Jenkins does not allow direct username changes, create a new user with the desired username:
  - Go back to **Manage Users** and click **Create User**.
  - Fill in the details for the new username, including a secure password and email address.
  - Assign the appropriate roles and permissions.

## 5. Change the Password
- If you want to change the password for an existing user:
  - Go to the user’s profile.
  - Look for an option to update the password (this might require a plugin if not visible by default).
  - Enter the new password and confirm it.
  - Save the changes.

## 6. Transfer Permissions (Optional Step)
- If you created a new user, ensure that the new user has the same roles and permissions as the old user.
- Use a Role-Based Access Control (RBAC) plugin if available to easily copy roles.

## 7. Remove or Disable the Old User
- To avoid confusion, go back to **Manage Users**.
- Either **Delete** or **Disable** the old user account.

## 8. Test the New User Account
- Log out and log in with the new username and password to verify that everything works correctly.

---

## Notes:
- Always back up your Jenkins configuration before making significant changes.
- Ensure the new username and password comply with your organization's security and naming policies.
- If using LDAP or another external authentication method, username and password changes might need to be handled externally.



# InfluxDB User Management via API
For InfluxDB, user management using the UI is not possible. Use the API as described below.

## List Users
```bash
curl -X GET http://localhost:8086/api/v2/users \
  -H "Authorization: Token <your_token>"
```

---

## Get the Organization ID
Send a `GET` request to the `/api/v2/orgs` endpoint to list organizations:

```bash
curl -X GET http://localhost:8086/api/v2/orgs \
  -H "Authorization: Token <your_admin_token>"
```

---

## Get Owners List
Use the following API to get the list of owners for an organization:

```bash
curl -X GET http://localhost:8086/api/v2/orgs/{org_id}/owners \
  -H "Authorization: Token <your_admin_token>"
```

---

## Add the User as an Owner
Use the `/api/v2/orgs/{org_id}/owners` endpoint:

```bash
curl -X POST http://localhost:8086/api/v2/orgs/{org_id}/owners \
  -H "Authorization: Token <your_admin_token>" \
  -H "Content-Type: application/json" \
  -d '{
        "id": "<user_id>"
      }'
```

---

## Create a User
Make a `POST` request to the `/api/v2/users` endpoint:

```bash
curl -X POST http://localhost:8086/api/v2/users \
  -H "Authorization: Token <your_token>" \
  -H "Content-Type: application/json" \
  -d '{
        "name": "<username>",
        "status": "active"
      }'
```

---

## Change Password
Use the `/api/v2/users/{user_id}/password` endpoint:

```bash
curl -X POST http://localhost:8086/api/v2/users/{user_id}/password \
  -H "Authorization: Token <your_token>" \
  -H "Content-Type: application/json" \
  -d '{
        "password": "<new_password>"
      }'
```

---

## Assign Admin Privileges
Grant admin privileges by assigning the `admin` role to the user:

```bash
curl -X PATCH http://localhost:8086/api/v2/users/{user_id} \
  -H "Authorization: Token <your_admin_token>" \
  -H "Content-Type: application/json" \
  -d '{
        "role": "admin"
      }'
```

---

## Assign the User to the New Role
Assign the user as an owner:

```bash
curl -X POST http://localhost:8086/api/v2/orgs/{org_id}/owners \
  -H "Authorization: Token <your_admin_token>" \
  -H "Content-Type: application/json" \
  -d '{
        "id": "<user_id>"
      }'



