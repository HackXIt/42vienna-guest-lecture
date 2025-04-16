# MANUAL_TESTING.md

## Manual Testing Methodologies

Manual testing involves testing software manually without automated tools, focusing on user interaction and system behavior.

### Methodologies for Saucedemo.com
- **Black-Box Testing:** Testing the application without internal knowledge of the code structure, from a user's perspective.
- **Exploratory Testing:** Informal testing without predefined test cases, suitable for discovering unexpected behaviors.
- **Acceptance Testing:** Ensuring features meet the specified requirements and end-user needs.

### Writing a Manual Test Case

#### Example:

- **Title:** Verify Successful Login
- **Precondition:** Valid user credentials
- **Steps & Results:**

| Steps                         | Expected Results                  |
|-------------------------------|-----------------------------------|
| Navigate to login page        | Login page loads successfully     |
| Enter valid username/password | Input is accepted                 |
| Click Login button            | User is redirected to inventory page |