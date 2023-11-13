## Other Important Recommendations

### Two-factor Authentication (2FA)

[Two-factor Authentication (2FA)](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication) adds an extra layer of security when logging into websites or apps. 2FA protects your account if your password is compromised by requiring a second form of authentication, such as codes sent via SMS or authentication app, or touching a physical security key.

We strongly recommend that you enable 2FA on GitHub and any important account where it is available. 2FA is not a Scorecard check because GitHub does not make that data about user accounts public. Arguably, this data should always remain private, since accounts without 2FA are so vulnerable to attack.

Though it is not an official check, we urge all project maintainers to enable 2FA to protect their projects from compromise.

#### Enabling 2FA

##### For users

Follow the steps described at [Configuring two-factor authentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication)

If possible, use either:

- physical security key (preferred), such as Titan or Yubikey
- recovery codes, stored in an access protected and encrypted vault

As a last option, use SMS. Beware: 2FA using SMS is vulnerable to [SIM swap attack](https://en.wikipedia.org/wiki/SIM_swap_scam).

##### For an organization

1. [Prepare to require 2FA in your organization](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-two-factor-authentication-for-your-organization/preparing-to-require-two-factor-authentication-in-your-organization)
2. [Require 2FA in your organization](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-two-factor-authentication-for-your-organization/requiring-two-factor-authentication-in-your-organization)
