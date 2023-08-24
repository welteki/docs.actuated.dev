# Register your GitHub Organisation

Would you like to try out Actuated for your team and GitHub Organisation?

## What you'll need

* A GitHub organisation
* One or more public or private repositories hosted in the organisation
* Administrative access to install the actuated GitHub App
* One or more bare-metal servers or VMs that support nested virtualisation (if you need a recommendation, just ask us)

## We'll guide you through the process

![Onboarding steps](/images/onboarding-steps.png)
> The onboarding process is easy, but we think it's best to talk to us so we can make it as simple as possible for you.

Actuated is a managed service or SaaS, where you bring your own servers to perform GitHub Actions. Just sign up for a plan, install the GitHub App on an organisation, and install the agent on one or more servers.

We've run almost 100,000 VMs for commercial teams already, and there's very little for you to do once set up, but we recommend a short call to get you started and to make sure that you'll get enough value from the service.

* [Book a call with us](https://forms.gle/8XmpTTWXbZwWkfqT6)

Before the call, you should run our [free actions-usage CLI tool](https://github.com/self-actuated/actions-usage) and send over the results via email. It'll help us recommend the best fit plan and server sizes for you.

## Install the GitHub App

Once accepted into the pilot program, an administrator from your GitHub organisation will need to install our GitHub App. GitHub Apps provide fine-grained access controls for third parties integrating with GitHub.

Learn more in the [FAQ](faq.md). 

!!! info "End User License Agreement (EULA)"
    Make sure you've read the [Actuated EULA](https://github.com/self-actuated/actuated/blob/master/EULA.md) before registering your organisation with the actuated GitHub App.

1. Click on the [Actuated Pilot GitHub App](https://github.com/apps/actuated-pilot)
2. Click **Install app**
3. Select the organisation you want to install the Actuated app to
4. Install the app on all repositories or select repositories

    ![Install GitHub app](/images/install_github_app.png)

5. Once installed you will will see the permissions and other configuration options for the Actuated app on your selected account. To install the app on another account you can repeat these installation steps.

To manage or uninstall the Actuated app navigate to "Settings" for your repository or organisation and click on "GitHub Apps" in the left sidebar. Select the Actuated app from the list and click "configure".

## Next steps

Now that you've installed the GitHub App, and picked a subscription plan:

* [Provision a server](/provision-server)
