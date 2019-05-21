def folders = ["config", "account-service", "auth-service", "gateway", "monitoring", "notification-service", "registry", "statistics-service", "turbine-stream-service"]

// The map we'll store the parallel steps in before executing them.
def stepsForParallel = folders.collectEntries {
    ["echoing ${it}" : transformIntoStep(it)]
}


def transformIntoStep(inputString) {
    // We need to wrap what we return in a Groovy closure, or else it's invoked
    // when this method is called, not when we pass it to parallel.
    // To do this, you need to wrap the code below in { }, and either return
    // that explicitly, or use { -> } syntax.
    return {
        node {
            checkout scm

            sh "cd ${inputString}; mvn clean package -DskipTests; cd .."

            acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
                registryName: env.ACR_NAME, 
                resourceGroupName: env.ACR_RES_GROUP, 
                local: "./${inputString}",
                dockerfile: "Dockerfile",
                imageNames: [[image: "$env.ACR_REGISTRY/${inputString}:$env.BUILD_NUMBER"], [image: "$env.ACR_REGISTRY/${inputString}:latest"]]
        }
    }
}


node('master') {
    stage('init') {
        checkout scm
    }

    stage('image build') {
        parallel stepsForParallel
    }

    stage('preview') {
        acsDeploy azureCredentialsId: env.AZURE_CRED_ID, 
            configFilePaths: "scripts/deployment/mq.yaml", 
            containerService: "$env.AKS_NAME | AKS",
            enableConfigSubstitution: true, 
            resourceGroupName: env.AKS_RES_GROUP,
            secretNamespace: env.TARGET_NAMESPACE,
            secretName: env.ACR_SECRET_NAME

        acsDeploy azureCredentialsId: env.AZURE_CRED_ID, 
            configFilePaths: "scripts/service/mq.yaml", 
            containerService: "$env.AKS_NAME | AKS",
            enableConfigSubstitution: true, 
            resourceGroupName: env.AKS_RES_GROUP

        for (int i=0; i<folders.size(); i++) {
            withEnv(["IMAGE_TAG=${BUILD_NUMBER}"]){
                acsDeploy azureCredentialsId: env.AZURE_CRED_ID, 
                    configFilePaths: "scripts/deployment/${folders[i]}.yaml", 
                    containerRegistryCredentials: [[credentialsId: env.ACR_CREDENTIAL_ID, url: "http://$env.ACR_REGISTRY"]],
                    containerService: "$env.AKS_NAME | AKS",
                    enableConfigSubstitution: true, 
                    resourceGroupName: env.AKS_RES_GROUP,
                    secretNamespace: env.TARGET_NAMESPACE,
                    secretName: env.ACR_SECRET_NAME

                acsDeploy azureCredentialsId: env.AZURE_CRED_ID, 
                    configFilePaths: "scripts/service/${folders[i]}.yaml", 
                    containerService: "$env.AKS_NAME | AKS",
                    enableConfigSubstitution: true, 
                    resourceGroupName: env.AKS_RES_GROUP
            }
        }
    }
}