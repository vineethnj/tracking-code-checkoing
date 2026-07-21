# TaskFlow — Zephra tracking test site (Purchase mode)

A tiny 3-page demo website with the Zephra tracking snippet already installed.
Use it to prove end-to-end that tracking + the **Purchase** conversion work.

```
index.html            Store / landing page  (CTAs → checkout)
checkout.html         Checkout form         (paying → order-confirmed.html)
order-confirmed.html  "Order confirmed" success page  ← this is the Purchase conversion
```

The snippet on every page (already embedded, your account):

```html
<script src="https://zephraai.com/sdk.js?cid=cli_Lu6h_2nI1kc&consent=auto" async></script>
```

---

## 1. Host it on GitHub Pages

1. Create a new GitHub repo (e.g. `taskflow-demo`) and upload these 4 files.
2. Repo **Settings → Pages → Build from branch → `main` / root → Save**.
3. After ~1 minute your site is live at either:
   - `https://<username>.github.io/taskflow-demo/`  (project site — a **sub-path**), or
   - `https://<username>.github.io/`  (if the repo is named `<username>.github.io`).

Both work — the order-confirmed page tells you the exact path to map (step 3).

---

## 2. Visit the site and generate activity

Open your live site and do a full run-through:

1. Open `index.html` — this fires a **page view**. *(No cookie banner is present,
   so the SDK auto-grants consent after ~3 seconds — wait a moment on each page
   so events send. This matches your real `consent=auto` snippet.)*
2. Click a button or two ("Buy TaskFlow Pro", "Watch demo") — these are captured as
   **button clicks**.
3. Click **Buy now** → fill the checkout form → **Pay ₹500**.
4. You land on **order-confirmed.html** — stay ~5 seconds. This page load is your
   **Purchase** conversion.

---

## 3. Map the Purchase conversion in Zephra

In Zephra → **Leads → Tracking Code → What do you want to count? → + Add a conversion**:

1. **What to count?** → **🛒 Purchase**
2. **How do we know?** → **They land on a page**
3. Paste the path shown **on your order-confirmed page** (the dashed box shows it, e.g.
   `/taskflow-demo/order-confirmed.html` on a project site, or `/order-confirmed.html`
   on a user site). Paste it **exactly**.
4. *(Optional)* **What is one worth?** → e.g. Same value ₹500
5. **Save conversion**

> The order-confirmed page prints its own path (`window.location.pathname`) in the
> dashed box, so you always paste the correct value — no guessing on GitHub sub-paths.

---

## 4. Confirm it works

- **Leads page → Tracking Code → Check your installation** → enter your site URL
  → **Check**. Should show ✅ *PASSED*. (Or use **Live check** and open the site.)
- Do another full purchase run.
- Back on the **Leads page** you'll now see a **🛒 Purchases** chip appear. Click
  it → your purchase shows as a person row with **✓ Counted** (and ₹500 if you set
  a worth). The email you typed appears there.
- **Website Activity** tiles (Page views / Button clicks / Purchases) go up.

---

## Notes

- **Only ONE Purchase per run.** The checkout form deliberately does not fire its
  own event (`window.AdsGenNoAutoForms = true` on checkout.html) so the sole
  Purchase signal is the order-confirmed page load you mapped — no double counting.
- **Nothing sensitive is sent.** The card number is never captured. Email/phone go to
  ad platforms only as hashed values, and only when you've connected + allowed it.
- Want to test **Leads** too? Remove the `AdsGenNoAutoForms` line from checkout.html
  and the form submit will be captured as a lead as well.
- This is a static demo — no real payment is taken; only the tracking is real.
