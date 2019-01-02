node {
    stage('init') {
        checkout scm
    }

    stage('build') {
         sh 'mvn clean package'
    }

    stage('image') {
        sh 'cd config'
        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.IMAGE_NAME:$env.BUILD_NUMBER"]]
    }

    stage('deploy') {
            acsDeploy azureCredentialsId: env.AZURE_CRED_ID, 
              configFilePaths: 'deployment.yml', 
              containerRegistryCredentials: [[credentialsId: env.ACR_CREDENTIAL_ID, url: "http://$env.ACR_REGISTRY"]],
              containerService: "$env.AKS_NAME | AKS",
              enableConfigSubstitution: true, 
              resourceGroupName: env.AKS_RES_GROUP,
              secretName: env.ACR_SECRET
    }
}