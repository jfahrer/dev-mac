# Manual checklist

Complete these tasks after `script/bootstrap` finishes.

1. **Protect the FileVault recovery key first.** If
   `~/Desktop/FileVault Recovery Key.txt` exists, move its key into secure
   off-device storage, verify that the stored copy is readable and exact, and
   securely delete the Desktop file. Do not keep the only recovery key on the
   encrypted Mac. Losing both the account password and recovery key can make
   the data unrecoverable.
2. Restart the Mac if FileVault, macOS updates, or changed settings require it.
3. Sign in to 1Password, browsers, Google Drive, Slack, and other accounts.
4. Grant Accessibility, Full Disk Access, Screen Recording, and other privacy
   permissions only to applications you trust.
5. Restore licenses for ScreenFlow, SwitchResX, Dash, and other paid apps.
6. Confirm SSH authentication and GitHub access.
7. In System Settings, configure Caps Lock as Control and remove unwanted
   Mission Control Control-arrow keyboard shortcuts.
8. Install the Alfred workflows and Dash docsets you want.
9. Confirm the menu-bar battery percentage and application-specific preferences
   that macOS does not expose reliably.
