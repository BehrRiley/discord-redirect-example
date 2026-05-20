# Discord Vanity Redirect via GitHub Pages

A lightweight, single-file template demonstrating how to create a custom vanity redirect URL for a Discord server using GitHub Pages and your DNS routing.

This repository serves as a live blueprint for routing community traffic from a custom sub-domain directly into a Discord communications hub without relying on third-party link shorteners.

## Features

* **Zero-Cost Hosting:** Leverages standard GitHub Pages infrastructure.
* **Instant Handshake:** Uses a dual-layered redirection strategy (HTML `<meta>` refresh + fallback JavaScript) to ensure the client is handed off instantly.
* **Aesthetic Preservation:** Features a dark background inline style block (`#05070a`) to prevent the white flashbang screen effect while the browser processes the redirect headers.

## Files Here
📁 Main Folder (Root)<br>
├── 📃`.nojekyll`    # Bypasses the default GitHub server-side Jekyll compiler<br>
├── 📃`favicon.ico`  # Not required; just provides an icon to your page<br>
└── 📃`index.html`   # Main routing file containing redirect targets and styles

> `.nojekyll` is intentionally meant to be a blank empty file.
> Remember to use the filename explicitly including the leading `.`.

## Implementation Guide

When working through the process of implementing this to a live URL connected to your domain, you can do this in any order *except* for modifying your page's `Enforce HTTPS` setting.

To replicate this setup for your own domain, follow these deployment steps:

### GitHub Deployment
1. You may easily fork, clone, or just copy the raw content out of this repository
2. Open `index.html` and replace all instances of the template invite link (`https://discord.gg/mpxGDSJkkW`) with your specific Discord server invite URL
3. Navigate to your repository `Settings` -> `Pages`
4. Set the `Source` to `Deploy from a branch` and choose your deployment branch (eg: `main`)

### DNS Configuration

To route your chosen sub-domain (eg: `discord.yourdomain.com`) to your GitHub Pages profile, you'll need to map a new record in your DNS dashboard. Here is how to manage it via CloudFlare and GoDaddy:

#### Using Cloudflare:

* **Type:** `CNAME`
* **Name:** `discord` (or your preferred sub-domain prefix)
* **Target:** `yourusername.github.io`
* **Proxy Status:** `DNS Only` (Grey Cloud)
  * *Note: Keep it unproxied during initial validation so GitHub can successfully verify ownership and provision the SSL certificate.*

#### Using Godaddy

GoDaddy handles standard DNS directly without an intermediate reverse-proxy layer like Cloudflare’s orange/grey cloud toggle. Because there is no proxy to disable, you do not have to worry about routing interference during GitHub's SSL verification; It works right out of the box.
1. Log in to your **GoDaddy Domain Portfolio**, select your domain, and select **Manage DNS**.
2. Click the **Add New Record** button.
3. Configure the fields with these exact parameters:
   * **Type:** `CNAME`
   * **Name:** `discord` *(or your preferred sub-domain prefix)*
   * **Value:** `yourusername.github.io`
   * **TTL:** `1 Hour` *(or default)*
4. Click **Save**.

> GoDaddy labels the destination field as `Value` or `Points to`, whereas Cloudflare labels it as `Target`.
> Cloudflare updates its edge servers almost instantly. GoDaddy's DNS updates can take anywhere from a few minutes to a few hours to fully propagate across the internet, meaning GitHub might take slightly longer to verify your domain changes.

### Last Step: Finalizing Custom Domain Settings
1. Return to your repository `Settings -> Pages`.
2. Under `Custom domain`, enter your full sub-domain address (eg, `discord.yourdomain.com`) and click `Save`.
3. Once the automated cryptographic DNS check completes, check the box for `Enforce HTTPS` to secure the connection path.
