# Email Spoofing

There are three key elements that together mitigate email spoofing on a
domain, and that should always be configured together.

- SPF
- DKIM
- DMARC

All of these are configured through DNS TXT records. They are described in
more detail [here](https://blog.ironbastion.com.au/email-impersonation-scams-phishing-what-your-staff-can-do/).

Google Cloud setup instructions can be found here:

- [SPF](https://support.google.com/a/answer/33786)
- [DKIM](https://support.google.com/a/answer/174124)
- [DMARC](https://support.google.com/a/answer/2466563)
