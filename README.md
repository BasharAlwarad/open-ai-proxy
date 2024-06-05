# Open AI Proxy

A simple reverse proxy for Open AI Chat Completions API

## Setup

- Fork repo
- Clone into your computer
- `cd` into working directory
- `npm i` to install dependencies
- create a `.env` file with a variable `OPEN_AI_APIKEY` with a value set to a valid Open AI API key, [find more about how to get yours here](https://platform.openai.com/api-keys)
- The server defaults to port `5050`, although an environment variable `PORT` can be used to override this behaviour

## Commands

- `npm run dev`: Starts development server, pulling environment variables from `.env` file
- `npm start`: Production server, environment variables need to be passed to system.

## Usage

The API expects 2 mandatory headers `provider` and `mode`

- Currently, `open-ai` is the only valid value for provider.
- `development` and `production` are accepted as values for `mode`

```javascript
const myHeaders = new Headers();
myHeaders.append('provider', 'open-ai');
myHeaders.append('mode', 'production');
myHeaders.append('Content-Type', 'application/json');

const raw = JSON.stringify({
  messages: [
    {
      role: 'system',
      content: 'You are a helpful assistant'
    },
    {
      role: 'user',
      content: 'Hello how are you?'
    }
  ],
  response_format: {
    type: 'json_object'
  },
  model: 'gpt-3.5-turbo'
});

const requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch('http://localhost:5050/api/v1/chat/completions', requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.error(error));
```

## Development vs Production mode

In production mode, requests will be proxied to Open AI via an instance of the OpenAI SDK, in development mode however, the requests is not proxied, instead, fake data is returned.
