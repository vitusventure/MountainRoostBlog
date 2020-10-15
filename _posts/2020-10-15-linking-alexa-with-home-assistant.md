---
layout: post
title: Linking Alexa with Home Assistant
---

While Home Assistant's app is a great way to manually control automations, I wanted to go full scifi home-from-the-future and add a voice assistant to the mix. Many people rightfully have privacy concerns regarding bringing smart speakers into their homes but I decided that the convenience of a voice assistant outweighed the concerns (real or imagined). Each person should make that determination on their own! Hopefully, there will be an open source alternative that is as easy as dropping some Echo Dots around the house in the future, but the open solutions are just not there yet.

## Home Assistant Cloud vs Rolling Your Own

For the easiest integration between Home Assistant and Alexa, you can just subscribe to Home Assistant's cloud solution. The price is relatively low ($5/month at this time), and it is a "just works" solution. However, if you are like me, there is some joy in building out your own solution which is free (or very close to free).

Home Assistant [provides some instructions](https://www.home-assistant.io/integrations/alexa.smart_home/) on setting up your own integration between your local instance and Alexa, but the instructions are very manual, especially when building out the AWS resources. There are a bunch of things to create and a bunch of things to think about if you decide to tear it down. Luckily, AWS provides just the service to remove the pain from this process!

## Home Assistant -> Alexa via Cloudformation

Cloudformation is an AWS service that lets you define AWS resources in a template. You hand a template to the service and it creates all the resources across the various AWS services. The resultant entity is called a "Stack". Once you are done with the resources, simply delete the stack and everything goes away. This is much nicer than manually creating a bunch of stuff. Just upload some YAML, provide some parameters, and all the AWS setup is done!

### Prerequisites

Your instance of Home Assistant must be externally accessible via HTTPS, with a valid SSL certificate. There are plenty of guides available for this online, and the certificate provided by LetsEncrypt will work!

You will also need an account for Amazon Developer and Amazon Web Services. These are free to create.

### Steps

First, follow the [official documentation](https://www.home-assistant.io/integrations/alexa.smart_home/#create-an-amazon-alexa-smart-home-skill) steps for creating an Amazon Alexa Smart Home Skill. This basically involves clicking through their UI to make your skill. At the end of those steps, you should have a Skill ID from Amazon, something like "amzn1.ask.skill.ed66dfa4-1185-492e-bf6e-1f70e90fb018".

When the official documentation gets to "Create an AWS Lambda Function", **STOP** and follow these directions:

* Log into your AWS account, then go to the Cloudformation console in us-east-1 (for English language skills).

* From the console, click "Create Stack".

* [Download the template](/assets/HAtoAlexaSmartHome.yaml) to your computer, then in the AWS console, for template source, select "Upload a template file" and select the template you just downloaded. Click "Next".

* Give the template a name (this is just something for you to recognize it as). In the parameters, fill in the skill ID you created earlier and the publicly accessible URL for your Home Assistance instance. Click "Next".

* Click "Next" on the stack options page.

* In the review page, scroll to the bottom and check the box indicating that Cloudformation may create IAM resources. The template includes an IAM role for your Lambda function to run as. Then click "Create Stack".

* Cloudformation will then provision all the resources you need to run your smart home skill and will automatically add the necessary code and triggers to the Lambda function. Wait for the stack to enter "CREATE_COMPLETE"

* Click the "Outputs" tab for your stack in Cloudformation and copy the FunctionARN key (it should be something like "arn:aws:lambda:us-east-1:1234567890:...")

From here, you can return to the official documentation at "[Configure the Smart Home Service Endpoint](https://www.home-assistant.io/integrations/alexa.smart_home/#configure-the-smart-home-service-endpoint)" to finish the skill setup and setup account linking. Use the ARN you copied from the Cloudformation Outputs as the Smart service endpoint.

For the component configuration in Home Assistant, I have something as simple as:

```
alexa:
    smart_home:
```

After that, simple run Device Discovery from your Alexa app, and you should see the devices passed through from Home Assistant!

## Conclusion

While there are still some manual tasks to run, using Cloudformation removes a bunch of tedious steps configuring AWS resources. As an added bonus, if you decide that you don't want to have this Alexa integration anymore, simply go to the Cloudformation console and click "Delete Stack". All the resources will be cleanly deleted!

I am still new with all of this, so I can't guarantee that everything is exactly as it should be. However, this process has been working fine for me and it is basically identical to the official steps, just packaged up a bit more nicely!
