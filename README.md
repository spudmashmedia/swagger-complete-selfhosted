# Swagger Complete  - Self Hosted
Docker Compose file to generate a complete Swagger Development environment (Editor + Generator)
No outbound http calls to * .swagger.io


## Requirements
Please install

1) Docker Toolbox
```
https://www.docker.com/products/docker-toolbox
```
2) Docker-Compose
```
https://docs.docker.com/compose/
```

## Installation

Clone the repository

Using Kitematic (Docker toolbox), click Docker CLI to open a console which is remoted into your Virtual Box VM with Docker.

In the console, run:
```
  /> docker-compose -f docker-compose.yaml up -d
```

At this point 2 docker containers will be created:
- swaggercompleteselfhosted_editor_1  (port 28099)
- swaggercompleteselfhosted_codegen_1 (port 28199)

Now remote into swaggercompleteselfhosted_editor_1 to update *defaults.json* file for the editorOptions
```
  /> docker exec -it swaggercompleteselfhosted_editor_1 sh
```
once remoted into the container:
```
  /editor> cd config
  /editor/config> vi defaults.json
```

Now replace
1) Replace generators.swagger.io host name to ip:port of container swaggercompleteselfhosted_codegen_1 (for this example host is 192.168.99.100, and IP is from our codegen container 28199):
```
"codegen": {
      "servers": "http://generator.swagger.io/api/gen/servers",
      "clients": "http://generator.swagger.io/api/gen/clients",
      "server": "http://generator.swagger.io/api/gen/servers/{language}",
      "client": "http://generator.swagger.io/api/gen/clients/{language}"
    },
```
to:
```
  "codegen": {
      "servers": "http://192.168.99.100:28199/api/gen/servers",
      "clients": "http://192.168.99.100:28199/api/gen/clients",
      "server":  "http://192.168.99.100:28199/api/gen/servers/{language}",
      "client":  "http://192.168.99.100:28199/api/gen/clients/{language}"
    },
```
2) Remove import proxy url (no outbound calls)
```
  "importProxyUrl": "https://cors-it.herokuapp.com/?url="
```
to
```
  "importProxyUrl": ""
```

## Test

### Test Editor
In your Chrome web browser, navigate to
```
http://192.168.99.100:28099
```

Swagger-editor should now load.

### Test Code Generator
With the editor still open, and developer network tab open, in the menu
1) Generate Server | Aspnet5

a zip file should start downloading.
in the network tab, you will see a HTTP POST to
```
http://192.168.99.100:28199/api/gen/clients/csharp
```

To verify the code generation is produced locally, In Kitematic under the swaggercompleteselfhosted_codegen_1 container you should see a log

```
looking for fileId 178b167b-641d-41d6-a396-c864df847e33
got filename /tmp/codegen-7652262760374524981-tmp/csharp-client-bundle.zip
```

this is getting generated from your local environment.


## Contributing
1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## History
- V1



## License
Copyright @ 2016 Spudmash Media Pty Ltd
