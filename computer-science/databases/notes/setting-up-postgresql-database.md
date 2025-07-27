[Back](../index.md)

# Setting up a PostgreSQL Database | Notes

### **Setting up Needed Schemas**
  - e.g ("auth" - for users table, "public" for CRUD applied data)
   
### **Setting up Needed Roles**
  - "postgres" - Default admin role with full privileges.
  - "anon" - For unauthenticated, public access (used by APIs when no user is logged in)
  - "authenticated" - For logged-in users accessing data via JWT.
  - "service_role" - Elevated access to bypas Row Level Security (RLS)
  - "authenticator" - Used to validate JWTs and switch to the correct user Roles

  **Create role command**
  ```
  -- Admin role
  CREATE ROLE postgres WITH LOGIN PASSWORD 'secure_password';

  -- Other 
  CREATE ROLE {role}
  ```
  
### **Granting Permissions**
  - "GRANT" is used to assign access to schemas, tables, or function.

  **Grant command**
  ```
  GRANT USAGE ON SCHEMA public TO anon;
  GRANT SELECT ON ALL TABLES IN SCHEMA public TO anon;

  GRANT USAGE ON SCHEMA public TO authenticated;
  GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO authenticated;
  ```
### **(Optional): Role Switching via JWT**
  - If you're using JWTs, you can configure your backend to switch roles based on claims.
  
  **e.g**
  ```
  SET ROLE authenticated;
  
  OR 
  
  -- During a transaction
  SET LOCAL role = 'authenticated'; 
  ```
  
