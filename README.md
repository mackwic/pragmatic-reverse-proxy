# Scalingo Api-Management

Buildpack:
- multibuildpack
- nginx
- rust: https://github.com/emk/heroku-buildpack-rust

### Todo:

#### Feature: Default is doc
- [ ] Default nginx file redirect all to HTML documentation page on how to set password (bonus: suggest a strong password)
#### Feature: API
- [ ] The API does not boot without the env var API_SECRET (should log an error)
- [ ] GET http://localhost/-/api without http basic auth api:$API_SECRET returns a 400
- [ ] GET http://localhost/-/api with the correct auth returns 200 OK with the API version
- [ ] When the config is empty, GET /-/api/reverse-proxy/entries return an empty list
- [ ] When the config has one entry (from x.scalingo.com/api to api.scalingo.com/), GET /-/api/reverse-proxy/entries returns a list of one element `[{from_dns: 'x.scalingo.com', to_dns: 'api.scalingo.com', from_location: '/api', to_location: '/' }]`
- [ ] When 
- [ ] Refuse the config when (from_dns == to_dns) && (from_location == to_location)
#### Feature: Persistant storage
- [ ] The nginx configuration is loaded from redis from the `nginx_configuration` key and deserialized as JSON
- [ ] The nginx configuration is persisted to redis in the `nginx_configuration` key and serialized as JSON
#### Feature: Config Generator (from the nginx configuration, generate a valid nginx config file)
- [ ] For each entry, generate:
    ```nginx
    location /$from_location {
        proxy_pass https://$to_dns/$to_location$uri;
        proxy_redirect https://$to_dns/$to_location https://$http_host/;
    }
    ```
#### Feature: Event bus
- [ ] At boot, the binary subscribe to the `change_events` channel
- [ ] When an event occurs on the channel, re-run the config generation, then send a SIGHUP to the Nginx process