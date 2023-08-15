# Register your GitHub Organisation

Would you like to try out Actuated for your team and GitHub Organisation?

## What you'll need

* A GitHub organisation
* One or more public or private repositories hosted in the organisation
* Administrative access to install the actuated GitHub App
* One or more bare-metal servers or VMs that support nested virtualisation (if you need a recommendation, just ask us)

## We can guide you through the process

Actuated is a managed service or SaaS, where you bring your own servers to perform GitHub Actions. Just sign up for a plan, install the GitHub App on an organisation, and install the agent on one or more servers.

We've run almost 100,000 VMs for commercial teams already, and there's very little for you to do once set up, but we recommend a short call to get you started.

[Schedule a call](https://forms.gle/8XmpTTWXbZwWkfqT6)

## Purchase a subscription

You can purchase a [subscription via our store on LemonSqueezy](https://openfaas-subscribe.lemonsqueezy.com/checkout/buy/6869822f-d5bd-40be-9b93-c45c25dcf2f1).

What's the right plan size?

You can get an idea of your expected usage by running our [free actions-usage CLI tool](https://github.com/self-actuated/actions-usage), or by roughly correlating the number of people committing to the repositories by 1-2x. So for a team of 5, you may want between 5-10 concurrent builds. As the concurrency increases in your plan, so do other features and benefits.

We recommend picking the right plan for your intended usage from the offset, to get the most out of your initial experience.

If you have any questions, contact us using the email address on the page, or reach out for a short call with the link below and we can answer any questions you may have.

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
