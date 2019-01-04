node {
    stage('init') {
        checkout scm
    }

    stage('build') {
         sh 'mvn clean package'
    }

    stage('image') {
        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './registry',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.REGISTRY_IMAGE_NAME:$env.BUILD_NUMBER"]]

        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './config',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.CONFIG_IMAGE_NAME:$env.BUILD_NUMBER"]]

        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './account-service',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.ACCOUNT_IMAGE_NAME:$env.BUILD_NUMBER"]]

        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './auth-service',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.AUTH_IMAGE_NAME:$env.BUILD_NUMBER"]]


        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './notification-service',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.NOTIFICATION_IMAGE_NAME:$env.BUILD_NUMBER"]]

        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './turbine-stream-service',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.TURBINE_IMAGE_NAME:$env.BUILD_NUMBER"]]


        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './gateway',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.GATEWAY_IMAGE_NAME:$env.BUILD_NUMBER"]]

        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './monitoring',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.MONITORING_IMAGE_NAME:$env.BUILD_NUMBER"]]

        acrQuickTask azureCredentialsId: env.AZURE_CRED_ID, 
            registryName: env.ACR_NAME, 
            resourceGroupName: env.ACR_RES_GROUP, 
            local: './statistics-service',
            dockerfile: "Dockerfile",
            imageNames: [[image: "$env.ACR_REGISTRY/$env.STATISTICS_IMAGE_NAME:$env.BUILD_NUMBER"]]
    }

    stage('deploy') {
            acsDeploy azureCredentialsId: env.AZURE_CRED_ID, 
              configFilePaths: 'scripts/registry.yaml,scripts/mq.yml,scripts/config.yaml,scripts/account-service.yaml,scripts/auth-service.yaml,scripts/gateway.yaml,scripts/monitoring,scripts/notification-service.yaml,scripts/statistics-service.yaml,scripts/turbine-stream-service.yaml', 
              containerRegistryCredentials: [[credentialsId: env.ACR_CREDENTIAL_ID, url: "http://$env.ACR_REGISTRY"]],
              containerService: "$env.AKS_NAME | AKS",
              enableConfigSubstitution: true, 
              resourceGroupName: env.AKS_RES_GROUP,
              secretName: env.ACR_SECRET
    }
}