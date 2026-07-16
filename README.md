# figprogramrewards
The FIG Program Rewards page
Secret phrase instead of member number. Very doable, and clause 4 explicitly permits it ("method, terms, and contents... subject to change at Issuer's sole discretion" — you lawyered yourself this right). Five spots, four in index.html and one critical one in the script:
In index.html:

CONFIG — change cardCode: "00000001" to your phrase, e.g. cardCode: "fine ill go"
The comparison — the current norm() only strips spaces/dashes, so "Fine Ill Go" ≠ "fine ill go" by capitalization. Make it forgiving by changing the norm line to: const norm = s => s.toLowerCase().replace(/[^a-z0-9]/g, ""); — now case, spaces, and punctuation all get ignored, so "FINE, I'LL GO!" matches.
The input field — in the <div id="entry"> block: change the label text from ENTER MEMBER NUMBER, change the placeholder, and delete inputmode="numeric" (that forces a number pad on phones — bad for typing words). Optionally reduce the input's letter-spacing:.35em in the CSS, which looks great for digits but reads oddly for words.
The error message — "Member number not recognized" → whatever fits, "The phrase is not recognized. See Clause 4."

In the Apps Script — this one's mandatory or everything silently breaks: the canon_() function strips all non-digits, so a text phrase becomes an empty string and every phrase would match every other phrase. Change its first line to:
var d = String(v).toLowerCase().replace(/[^a-z0-9]/g, '');
(matching the site's new norm), and remove the leading-zeros line since it no longer applies — the function body becomes just that line plus return d === '' ? '0' : d;. Then the ritual: Deploy → Manage deployments → ✏️ → New version → Deploy, or the live script keeps running digit-logic.
