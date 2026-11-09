Fix '&apos' corruption

  PROBLEM:

HTML entities, specifically &amp;apos;which represents an apostrophe ' is
displayed as

&amp;apos;some-text&amp;apos;

CORRUPTED EXAMPLE:

&amp;apos;systemd-journal&amp;apos;

CORRECTED EXAMPLE:

Remove each instance of &amp;apos;and replace with an apostrophe

'systemd-journal'