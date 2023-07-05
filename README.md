# Mailjet Module

The nestjs-mailjet module provides a simple wrapper for the Mailjet API,
allowing NestJS applications to easily send transactional emails using the
Mailjet email delivery service.

## Installation

You can install the nestjs-mailjet module using npm:

```bash
npm install --save @adamkac/nestjs-mailjet
````

## Getting Started

To use the nestjs-mailjet module, you first need to import it into your NestJS
application. You can do this by adding the MailjetModule to your app's imports array:

```ts
import { MailjetModule } from '@adamkac/nestjs-mailjet';

@Module({
  imports: [
    MailjetModule.forRoot({
      apiKey: 'your_api_key',
      apiSecret: 'your_api_secret',
    }),
  ],
})
export class AppModule {}
```

The forRoot method takes an options object with the following properties:

- `apiKey`: The Mailjet API key.
- `apiSecret`: The Mailjet API secret.
- `sandboxMode` (optional): A boolean value indicating whether to enable sandbox mode. Default is `false`.

This module options extends the mailjet `ClientParams` options. You can find more [here](https://www.npmjs.com/package/node-mailjet).

## Injecting the MailjetService

Once the MailjetModule is imported, you can inject the MailjetService into your controllers or services:

```ts
import { Injectable } from '@nestjs/common';
import { MailjetService } from '@adamkac/nestjs-mailjet';

@Injectable()
export class EmailsService {
  constructor(private readonly mailjetService: MailjetService) {}

  async sendEmail(data: any) {
    const { from, to, subject, html, text } = data;

    await this.mailjetService.send({
      from,
      to,
      subject,
      html,
      text,
    });
  }
}
```


```ts
import { Injectable } from '@nestjs/common';
import { MailjetService } from '@adamkac/nestjs-mailjet';

@Injectable()
export class EmailsService {
  constructor(@Inject('MAILER_CLIENT') private readonly mailjetService: MailjetService) {}

  async sendEmail(data: any) {
    const { from, to, subject, html, text } = data;

    await this.mailjetService.send({
      from,
      to,
      subject,
      html,
      text,
    });
  }
}
```

## Using Templates

You can also use email templates with the MailjetService by passing a templateId and context:

```ts
await this.mailjetService.send({
  from,
  to,
  subject,
  context: {
    templateId: 12345,
    name: 'John Doe',
    orderNumber: 12345,
  },
});
```

## API

The MailjetService provides a single method for sending email:

### send

```ts
send(params: SendEmailParams): Promise<any>
````

Sends an email using the Mailjet API.

| Property | Type | Description |
| --- | --- | --- |
| `params` | `SendEmailParams` | The email parameters. |

The SendEmailParams interface has the following properties:

```ts
interface SendEmailParams {
  from: string;
  to: string | string[];
  subject?: string;
  html?: string;
  text?: string;
  context?: Record<string, any> & {
    templateId?: number;
  };
}
```
