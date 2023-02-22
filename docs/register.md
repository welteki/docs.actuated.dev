# Register your GitHub Organisation

Would you like to try out Actuated for your team and GitHub Organisation?

## What you'll need

* A GitHub organisation (you can also create a test organisation to try actuated out)
* One or more public or private repositories hosted in the organisation
* Administrative access to add our GitHub App
* One or more bare-metal hosts or VMs that support nested virtualisation (if you need a recommendation, feel free to ask)

## Sign-up for the pilot

Actuated is a managed service. We run a SaaS platform, integration with GitHub and VM images. All you need to do, is to install our agent on a number servers.

What is a pilot?

Actuated is already being used by commercial teams to run their pipelines, so why do we need a pilot? The pilot is a way for us to:

* build the right thing with customer feedback from day one
* land on the right pricing model
* find companies that are excited to work with us on solving this problem

[Sign-up for the pilot](https://forms.gle/8XmpTTWXbZwWkfqT6)

## Install the GitHub App

Once accepted into the pilot program, an administrator from your GitHub organisation will need to install our GitHub App. GitHub Apps provide fine-grained access controls for third parties integrating with GitHub.

Learn more in the [FAQ](faq.md). 

!!! info "End User License Agreement (EULA)"
    Make sure you've read the [Actuated EULA](https://github.com/self-actuated/actuated/blob/master/EULA.md) before registering your organisation with the actuated GitHub App.

1. Open the link that you received to install our GitHub App
2. Click **Install app**
3. Select the organisation you want to install the Actuated app to
4. Install the app on all repositories or select repositories

    ![Install GitHub app](/images/install_github_app.png)

5. Once installed you will will see the permissions and other configuration options for the Actuated app on your selected account. To install the app on another account you can repeat these installation steps.

To manage or uninstall the Actuated app navigate to "Settings" for your repository or organisation and click on "GitHub Apps" in the left sidebar. Select the Actuated app from the list and click "configure".

## Next steps

Now that you've installed the GitHub App, and picked a subscription plan:

* [Provision a server](/provision-server)
