# AWS Services - IAM Hands-On Example

## Scenario: Create a Developer User with Limited Permissions

This walkthrough demonstrates how to set up an IAM user, assign it to a group, attach permissions, and log in using an
account alias.

---

## 1. Create an IAM Group

1. Open the **IAM Console** → Groups → **Create New Group**.
2. Name it: `Developers`.
3. Attach a managed policy: **AmazonEC2ReadOnlyAccess**.
    - This allows developers to view EC2 instances but not modify them.

---

## 2. Create an IAM User

1. Go to **IAM Console** → Users → **Add User**.
2. Name: `dev-john`.
3. Select **AWS Management Console access**.
4. Choose **Auto-generate password** (optionally require password reset).

---

## 3. Add User to Group

1. On the next step, assign the user `dev-john` to the **Developers** group.
2. This gives the user read-only permissions for EC2.

---

## 4. Set Account Alias

1. In the IAM Console → Dashboard → **Account Alias** → Create alias.  
   Example: `mycompany`.
2. New login URL becomes:  
   `https://mycompany.signin.aws.amazon.com/console`

---

## 5. Log In as the User

1. Share the login URL with `dev-john`.
2. User signs in with username `dev-john` and the auto-generated password.
3. On first login, the user must reset their password.

---

## 6. Verify Permissions

- `dev-john` can open the EC2 console and **view instances**.
- If the user tries to start or stop an instance, access will be **denied**.

---

## 7. Summary

- IAM Groups help manage permissions at scale.
- Account Aliases simplify user login.
- Always follow **least privilege** and use **MFA** for added security.
