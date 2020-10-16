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

* Use one of the links below to create your stack (based on your language choice)
   * English (US), English (CA)
      * [Create your stack in us-east-1](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://home-assistant-alexa-cloudformation.s3.amazonaws.com/HAtoAlexaSmartHome.yml&stackName=HomeAssistantToAlexaBridge)
   * English (UK), English (IN), German (DE), Spanish (ES) or French (FR)
      * [Create your stack in eu-west-1](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?templateURL=https://home-assistant-alexa-cloudformation.s3.amazonaws.com/HAtoAlexaSmartHome.yml&stackName=HomeAssistantToAlexaBridge)
   * Japanese and English (AU)
      * [Create your stack in us-west-2](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?templateURL=https://home-assistant-alexa-cloudformation.s3.amazonaws.com/HAtoAlexaSmartHome.yml&stackName=HomeAssistantToAlexaBridge)
   * You can also grab the raw template [here](https://home-assistant-alexa-cloudformation.s3.amazonaws.com/HAtoAlexaSmartHome.yml).

* Fill in the two parameters, using your skill ID from earlier and the full URL to your instance of Home Assistant.
* Check the box to enable IAM resource creation. 
  * The stack has a single IAM role for the Lambda function, which only has access to CloudWatch Logs.
* Cloudformation will then provision all the resources you need to run your smart home skill and will automatically add the necessary code and triggers to the Lambda function. Wait for the stack to enter "CREATE_COMPLETE"
* Click the "Outputs" tab for your stack in Cloudformation and copy the FunctionARN key (it should be something like "arn:aws:lambda:us-east-1:1234567890:...")

From here, you can return to the official documentation at "[Configure the Smart Home Service Endpoint](https://www.home-assistant.io/integrations/alexa.smart_home/#configure-the-smart-home-service-endpoint)" to finish the skill setup and setup account linking. Use the ARN you copied from the Cloudformation Outputs as the Smart Home service endpoint.

For the component configuration in Home Assistant, I have something as simple as:

```
alexa:
    smart_home:
```

After that, simply run Device Discovery from your Alexa app, and you should see the devices passed through from Home Assistant!

## Conclusion

While there are still some manual tasks to run, using Cloudformation removes a bunch of tedious steps configuring AWS resources. As an added bonus, if you decide that you don't want to have this Alexa integration anymore, simply go to the Cloudformation console and click "Delete Stack". All the resources will be cleanly deleted!

I am still new with all of this, so I can't guarantee that everything is exactly as it should be. However, this process has been working fine for me and it is basically identical to the official steps, just packaged up a bit more nicely!
