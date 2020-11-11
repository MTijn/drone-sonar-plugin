# drone-sonar-plugin
The plugin of Drone CI to integrate with SonarQube (previously called Sonar), which is an open source code quality management platform.

Detail tutorials: [DOCS.md](DOCS.md).

### Build process
build go binary file: 
`GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o drone-sonar`

build docker image
`docker build -t aosapps/drone-sonar-plugin .`


### Testing the docker image:
```commandline
docker run --rm \
  -e DRONE_REPO=test \
  -e PLUGIN_SOURCES=. \
  -e SONAR_HOST=http://localhost:9000 \
  -e SONAR_TOKEN=60878847cea1a31d817f0deee3daa7868c431433 \
  aosapps/drone-sonar-plugin
```

### Pipeline example
```yaml
steps
- name: code-analysis
  image: aosapps/drone-sonar-plugin
  settings:
      sonar_host:
        from_secret: sonar_host
      sonar_token:
        from_secret: sonar_token
      key: some_project_key
```
### Available settings
- key: The project key the project is going to be build with
- sonar_host: The SonarQube host
- sonar_token: The token to authenticate to the SonarQube host with
- timeout: The web request timeout, standard set to 60 seconds
- sources: The sources that have to be scanned, comma separated
- inclusions: SonarQube property, extends the sources property
- exclusions: The source files that should not be analysed, comma separated
- level: The log level used during analysing the source code
- showProfiling: Show profiling information during scanning
- branchAnalysis: Only used with a licensed SonarQube version
- usingProperties: If set to true, the settings will be overridden by the sonar.properties file
