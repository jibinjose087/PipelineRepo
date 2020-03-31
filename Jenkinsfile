pipeline {
    agent any

    environment {
        IMAGE_NAME = "mymaven" 
    }

    stages {
        stage("Build docker images") {
            steps {
                script {
                    def versions = readYaml file: "versions.yml" 

                    def stages = versions.images.collectEntries { label, props -> 
                        [(label): {
                            stage(label) {
                                sh """docker build \
                                --build-arg JAVA_VERSION=${props.java} \
                                --build-arg MAVEN_VERSION=${props.maven} \
                                ${collectTags(props.tags, env.IMAGE_NAME)} \
                                .
                            """
                            }
                        }]
                    }

                    parallel stages 
                }
            }
        }
    }
}

@NonCPS
String collectTags(final List<String> tags, final String imageName) {
    return tags.collect { tag -> "-t ${imageName}:${tag}" }.join(" ")
}
