# Other Projects

A collection of my past university and personal projects, each in its own folder. Open any folder to see that project's full README.

\---

## 🎵 [The Light House](./TheLightHouse)

A concert venue booking system with three portals:

* **Customers** book tickets by zone, top up their balance and cancel bookings with automatic refunds.
* **Performers** request concert sessions and equipment.
* **Admins** approve sessions, review bookings and ban or unban users.

Rules such as zone capacity (200 per zone), no overlapping sessions and a 7-day cancellation window are enforced by MySQL triggers and stored procedures.

**Tech:** PHP, MySQL, HTML, CSS

**Run it:** Copy the folder into MAMP/XAMPP's `htdocs`, import `thelighthouse (4).sql` into a database named `thelighthouse`, update `dbconn.php` if needed, set `short\\\\\\\\\\\\\\\_open\\\\\\\\\\\\\\\_tag = On` in `php.ini`, then open `UI\\\\\\\\\\\\\\\_1/login.php`.

\---

## 📧 [Maily Detector](./maily-detector)

A Chrome extension that scans the email you have open in Gmail and uses an AI model to classify it as **spam**, **not spam** or **subscription**, with a confidence score and explanation. It also warns when an email is mostly Thai, since the model was trained on English.

**Tech:** JavaScript, Chrome Extension (Manifest V3), Hugging Face Spaces, Gradio API

**Run it:** Go to `chrome://extensions`, turn on **Developer mode**, click **Load unpacked** and select the `maily-detector` folder. Open an email in Gmail, click the extension and press **Scan**.

> The scanned email text is sent to a Hugging Face Space for classification.



