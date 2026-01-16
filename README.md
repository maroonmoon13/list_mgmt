# list_mgmt
Test app to manage Etsy listings on personal shop


# Notes from gemini on what to do:

#Phase 1: Preparation

    Register your App: Go to the Etsy Developer Portal and create an app.

    Get your Keystring: Copy the Client ID (also called the "keystring"). You generally do not need the "Shared Secret" for the PKCE flow.

    Set Redirect URI: In your app settings on Etsy, add a Redirect URI (e.g., https://localhost:3000/callback or a tool like Postman's callback URL).

Phase 2: Generate PKCE Codes

Etsy requires two values for security: a Code Verifier and a Code Challenge.

    Code Verifier: A random string (43–128 characters).

    Code Challenge: A Base64URL-encoded SHA256 hash of your verifier.

    Pro-Tip: If you just want to test manually without writing code yet, use an online PKCE generator to get these two strings instantly.

Phase 3: The Authorization Flow
Step 1: Construct the Authorization URL

Paste this URL into your browser, replacing the placeholders with your info:

https://www.etsy.com/oauth/connect?response_type=code&redirect_uri=YOUR_REDIRECT_URI&scope=listings_r%20listings_w&client_id=YOUR_CLIENT_ID&state=superstate&code_challenge=YOUR_CODE_CHALLENGE&code_challenge_method=S256

    Scopes: listings_r and listings_w allow you to read/write listings. Use spaces (encoded as %20) to add more.

    State: Use any random string to prevent CSRF attacks.

Step 2: Grant Access

The browser will ask you to log in to your Etsy shop and "Allow Access." Once you click allow, you will be redirected to your Redirect URI. Look at the URL in your address bar; it will now contain a code: https://your-redirect-uri.com/?code=abc123...
Step 3: Exchange Code for Access Token

You now have 60 seconds to exchange that code for an actual token. Use an API client like Postman or a curl command to send a POST request:

    URL: https://api.etsy.com/v3/public/oauth/token

    Body (x-www-form-urlencoded):

        grant_type: authorization_code

        client_id: YOUR_CLIENT_ID

        redirect_uri: YOUR_REDIRECT_URI

        code: THE_CODE_FROM_STEP_2

        code_verifier: YOUR_ORIGINAL_VERIFIER_STRING

Results

You will receive a JSON response containing:

    access_token: Used in the header of all your API calls (Authorization: Bearer [token]).

    refresh_token: Used to get a new access token once the first one expires (usually after 1 hour).
