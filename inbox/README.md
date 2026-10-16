   # inbox

   Drop ONE zip here (Add file → Upload files) and the `inbox` workflow
   unpacks it: every file goes to the path it carries inside the zip, the
   zip is removed, and it all lands as one commit. Paths are relative to the
   repo root (index.html, profile.html, Ian_Ingram_Resume.html).
   It refuses, and changes nothing, if any path is absolute, contains "..",
   or points into .github/ or inbox/. Watch it run under the Actions tab.

   This README keeps the folder alive between deliveries; leave it here.
