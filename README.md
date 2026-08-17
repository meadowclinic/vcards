# HOW TO: Create a "Digital Business Card" (NFC)

**Mike Scheinberg** / COO, Meadow Reproductive Health and Wellness<br>
[mscheinberg@meadowrepro.org](mailto:mscheinberg@meadowrepro.org)

**Want to do this for your clinic / organization?** Feel free to read below to find out how I made these for our staff.

> **If you'd rather skip the DIY project and want me to partner with you to make these — [feel free to reach out by email!](mailto:mscheinberg@meadowrepro.org)**

You may have noticed NFC "buttons" on staff name badges. You "tap" your phone to the button, and it prompts you to open a website which provides a "Contact Card" that can be saved to your personal device. It's very similar to a QR code — your phone reads it and converts it into a web address. The difference is the underlying technology: NFC (Near Field Communication) transfers a short amount of data over a very short distance — similar to how contactless phone payments work.

There are many ways to do this — this guide shows what I did, prioritizing a professional look and low project cost. I'll mention the vendors I used, but they're not the only option. Feel free to reach out with questions or suggestions.

> **A note on privacy:** This method makes your contact information available at a public URL. That means there's a remote, non-zero chance someone could stumble on it through a web search — especially if you skip the `robots.txt` step below. It's unlikely, but not impossible.
>
> Think of it like handing out a paper business card: if you drop it, there's a small chance someone unintended finds it. There's always some risk — but it's low. If you want a 0% chance of your card getting into the wrong hands, the only real option is not giving out a business card at all.

## Here are the steps

1. [Creating a VCARD](#1-creating-a-vcard) — the file that holds your contact information.
2. [Hosting your VCARD](#2-hosting-your-vcard) — making a URL where your VCARD is accessible.
3. [Getting / Writing an NFC Tag](#3-getting--writing-an-nfc-tag) — buy tags, get the app, write the URL, lock it with a password.

---

## 1. Creating a VCARD

VCARD is the standard format for digital contacts, primarily used on mobile devices. Some websites will let you enter your info and generate a `.vcf` (VCARD file) for you. You can also just create a text file (e.g. with Notepad).

Here's an example card:

```
BEGIN:VCARD
VERSION:4.0
N:White;Betty;;;
FN:Betty White
ORG:Cedars of St. Olaf Hospital
TITLE:Legendary Star and Badass Woman
EMAIL;TYPE=WORK:bwhite@cedarsstolaf.org
TEL;TYPE=WORK,VOICE:+15075550142
TEL;TYPE=CELL,VOICE:+15075550187
ADR;TYPE=WORK:;;2400 Herring Circle\n Suite 301;St. Olaf;MN;55057;USA
URL:https://glaad.org/tag/betty-white/
NOTE:Pronouns: she/her
END:VCARD
```

A few important notes:

- **(a)** The `BEGIN:VCARD` / `END:VCARD` tags are required — they define the file type.
- **(b)** The placement of semicolons (`;`) matters. Match the number and position shown above. To include credentials, add them *after* the three semicolons in the `N:` line — e.g. `White;Betty;;;FNP-C`. In `FN:`, add credentials without a leading comma — e.g. `Betty White APRN, CNM`.
- **(c)** The `\n` in the address line is a line break, separating a suite/unit number from the primary address. Use as needed.
- **(d)** Pronouns aren't yet a formal VCARD standard field — the `NOTE:` line is used here instead.
- **(e)** It's possible to embed a photo in the card, but it's more involved. See the [icons note](#a-quick-note-on-icons) below, or just reach out.

Save the file locally with a `.vcf` extension. (On Windows Notepad, change "Save as type" from `Text file (*.txt)` to `All Files` so it doesn't append `.txt`.) For example: `bettywhite.vcf`.

## 2. Hosting your VCARD

There are several ways to host a VCARD — you're essentially putting it somewhere on the web so it's reachable at an address. A few options: a hosted `.vcf` generator site, Google Drive, Dropbox, or your own clinic website.

I host ours on [GitHub](https://github.com) for a few reasons: (a) I'm a big nerd; (b) it's a reputable, free platform; (c) it lets me build an address that includes our organization's name. Here's how:

1. Go to [github.com/signup](https://github.com/signup) and create a free account. Pick a username you'd want reflected in your web address.
2. Follow the prompts to create a new repository — call it something like `vcards`.
3. **Add file → Upload file** → upload your `.vcf` to the repository.
4. Under **Settings → Pages**, enable GitHub Pages for the repo (source: `main` branch, root folder).

Once set up, the file becomes reachable at an address like:

**https://meadowclinic.github.io/vcards/bettywhite.vcf**

### Privacy: setting up `robots.txt`

This step is optional but strongly recommended — it asks well-behaved search engines not to index or crawl your hosted files, so they're far less likely to turn up in a random web search.

1. In your repository, go to **Add file → Create new file**.
2. Name the file `robots.txt`.
3. Add these two lines:
   ```
   User-agent: *
   Disallow: /
   ```
4. Commit the file.

**Important — location matters.** `robots.txt` only works if it sits at the *root* of your domain, not inside a subfolder. For a project site like this one, that means it needs to live in a separate repository named exactly `<yourusername>.github.io` (e.g. `meadowclinic.github.io`), with `robots.txt` in *that* repo's root — **not** inside your `vcards` repo. Once it's in the right spot, you can confirm it's working by visiting:

**https://meadowclinic.github.io/robots.txt**

You should see the same two lines served back to you.

**Be honest with yourself about what this does and doesn't do.** `robots.txt` is a *request*, not a lock — it only stops crawlers that choose to honor it (Google, Bing, etc. generally do). It does nothing against someone deliberately trying to scrape or guess file names. If you want stronger protection against targeted access, see [Advanced: non-guessable filenames](#advanced-non-guessable-filenames) below.

## 3. Getting / Writing an NFC Tag

NFC tags are inexpensive — a 24-pack typically runs under $10 on Amazon. **NTAG215** is a standard, reliable choice.

1. **Get the NFC Tools app** — free on iOS/Android, with an in-app purchase (around $3) to unlock tag writing.
2. **Add a record** — from the main screen: Write → Add a Record → URL/URI. Enter your card's web address *without* the `https://` prefix (e.g. `meadowclinic.github.io/vcards/bettywhite.vcf`), but make sure the record type at the top is set to `https://`.
3. **Write the tag** — tap Write, hold the NFC tag to the top of your phone, and let it finish.
4. **Test it** — tap the tag with your phone. You should get a prompt to open the link; tapping it should open the card.
5. **Password-protect the tag** — this prevents accidental or intentional overwriting. In NFC Tools: main menu → Other → Set Password. Choose something memorable — there's no password recovery.

## 4. Presentation

Personalizing the tags gives them a more finished look — small round stickers (about 1", matte finish) affixed directly to the tag work well and give a slightly raised, tactile feel.

One practical note: NFC doesn't read reliably through metal. Tags on metal surfaces (like a metal water bottle) likely won't scan well.

### A quick note on icons

It's possible to embed a small photo or logo directly into a `.vcf` file as a Base64-encoded image, so it shows up as the contact's picture. It works, but it can bloat the file size and slow down loading (or fail to load at all) if it's not handled carefully.

**Rather than turn this into a full how-to, this one's easiest as a conversation — if you'd like a custom icon or logo embedded in your cards, let me know and I'll work with you on it directly.**

## 5. Customization / Other uses

- The same technique works for things beyond contact cards — e.g. a Google Review sign that taps through to your review page. The setup is identical; only the "payload" URL changes.
- There may be a way to write a full VCARD directly into the tag itself, so contact info never touches the internet at all. I haven't found a reliable way to do this yet — let me know if you have.

---

<details>
<summary><strong>Advanced: non-guessable filenames</strong> (click to expand)</summary>

If you have many cards hosted in one repository, using real names as filenames (e.g. `bettywhite.vcf`) means anyone who finds the repo can browse and collect *all* of them at once — even with `robots.txt` in place, since it only deters compliant crawlers, not a person clicking around.

A stronger option: rename each file to a short random token instead — e.g. `k3m9xr2p.vcf` — so filenames can't be guessed or listed meaningfully.

**Trade-offs to know before doing this:**

- Any NFC tag already written with the old URL will break and need to be rewritten with the new one.
- You'll need somewhere to track which token belongs to which person — a private spreadsheet kept *outside* the repo works well. Never commit that mapping to the repo itself, or you've defeated the purpose.
- This adds real maintenance overhead. It's worth it for a larger organization with many cards; probably overkill for a handful of staff.

</details>

---

**Big favor for you:** I'm sharing this info in the spirit of helping out those clinics and orgs who are helping to make abortion accessible to those who seek it. If you're grateful for this info and want to say thanks:

1. You can email me! See above.
2. You can make a donation to Grace Reproductive Fund, the organization that funds Meadow Reproductive Health and Wellness → [meadowrepro.org/donate](https://meadowrepro.org/donate)
