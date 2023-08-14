# Register your GitHub Organisation

Would you like to try out Actuated for your team and GitHub Organisation?

## What you'll need

* A GitHub organisation (you can also create a test organisation to try actuated out)
* One or more public or private repositories hosted in the organisation
* Administrative access to add our GitHub App
* One or more bare-metal hosts or VMs that support nested virtualisation (if you need a recommendation, feel free to ask)

## Purchase a subscription

You can purchase a [subscription via our store on LemonSqueezy](https://openfaas-subscribe.lemonsqueezy.com/checkout/buy/6869822f-d5bd-40be-9b93-c45c25dcf2f1).

You can get an idea of your expected usage by running our free actions-usage CLI tool, or by roughly correlating the number of people committing to the repositories by 1-2x. So for a team of 5, you may want between 5-10 concurrent builds. Higher plans come with additional benefits.

We recommend picking the right plan for your intended usage from the offset, to get the most out of your initial month's subscription.

If you have any questions, contact us using the email address on the page, or reach out for a short call with the link below and we can answer any questions you may have.

## Want to talk to us first?

Actuated is a managed service. We run a SaaS platform, integration with GitHub and VM images. All you need to do, is to install our agent on a number servers.

Actuated is already being used by commercial teams to run their pipelines, but if you'd like to talk to us before going ahead with the platform, you can setup a call with us here:

[Schedule a call](https://forms.gle/8XmpTTWXbZwWkfqT6)

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
