# UroFollow deployment

## 1. Create Supabase project
Create a project at https://supabase.com/ and open its SQL Editor.

## 2. Create the schema
Run all SQL in `supabase/schema.sql`.

## 3. Create your first Auth user
In Supabase: Authentication → Users → Add user.
Create an email/password user for the clinic administrator.

## 4. Create clinic and profile
In SQL Editor, replace the values below and run:

```sql
insert into public.clinics (name) values ('My Urology Clinic') returning id;
```

Copy the returned clinic UUID, then run:

```sql
insert into public.profiles (id, clinic_id, full_name, role)
values (
  'AUTH_USER_UUID_HERE',
  'CLINIC_UUID_HERE',
  'Clinic Administrator',
  'admin'
);
```

For additional staff, create their Auth users and insert one profile row for each user using the same clinic UUID.

## 5. Configure the browser app
Open `index.html` and find:

```js
const CONFIG={url:"",key:""};
```

Set:
- `url` to your Supabase project URL
- `key` to your Supabase publishable/anon key

Never put a Supabase service-role/secret key in this file.

## 6. Deploy
The app is a static site. Upload `index.html` to a static host such as:
- Vercel
- Netlify
- Cloudflare Pages
- GitHub Pages (with appropriate configuration)

For Vercel: import the GitHub repository and deploy with no build command and `index.html` as the entry point.

## 7. Test before real use
Use dummy patient records first.

Test this exact workflow:
1. Create a patient.
2. Create a follow-up episode.
3. Add required investigations.
4. Link each investigation to the episode using the `episode_id` field if you are using the SQL/API directly.
5. Mark results received one by one.
6. Confirm the episode stays `waiting` until all required investigations are received.
7. Confirm it becomes `ready` when all are received.
8. Confirm `contact_early` can move it to contact.
9. Test stent overdue highlighting.

## Clinical/production note
This is a software starter implementation, not a validated medical-device or clinical decision-support product. Before entering identifiable patient information, have your organization review privacy, access control, retention, audit, backup, hosting, regulatory and clinical-governance requirements. Test the application and database security independently.
