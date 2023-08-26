# Java CI/CD Pipeline using GitHub Actions
This is  a basic general CI/CD pipeline for Java Apps using Github actions. This repository provides a Continuous Integration and Continuous Deployment (CI/CD) pipeline for Java developers using GitHub Actions. 
The pipeline automates the process of building, packaging, and deploying Java applications.

## Workflow Overview
The workflow defined in this repository is triggered on every push to the develop and main branches. It consists of two jobs: `build` and `deploy`.


### Build Job

The `build` job runs the following steps:

1. **Checkout Repository**: This step uses the `actions/checkout` action to fetch the latest code from the repository.

2. **Set up JDK 17**: This step uses the `actions/setup-java` action to configure JDK 17 for the build. It also caches Maven dependencies.

3. **Build and Package**: This step runs the `mvn clean package` command to build and package the Java application using Maven.

4. **Create Staging Directory**: This step creates a `staging` directory and copies the generated JAR files into it.

5. **Upload Artifact**: The `actions/upload-artifact` action is used to upload the contents of the `staging` directory as an artifact named "Package". This artifact is then used in the deployment job.

### Deploy Job

The `deploy` job is dependent on the successful completion of the `build` job. It deploys the application to a remote server using SSH:

1. **Configure SSH**: This step sets up SSH key-based authentication for connecting to the deployment server. The private SSH key is stored as a secret and is used to authenticate the deployment.

2. **Download JAR**: The `actions/download-artifact` action is used to download the previously uploaded artifact from the `build` job.

3. **Copy JAR to VM**: This step uses the `scp` command to copy the downloaded JAR file to the deployment server. It requires the previously configured SSH key for authentication.

4. **Deploy Application**: The final step of the deployment job connects to the deployment server using SSH and performs the necessary actions to deploy the application. In this example, it reloads the systemd daemon, stops the existing application service, and starts the new version of the application.

### Getting Started

To set up this CI/CD pipeline for your Java project, follow these steps:

1. **Fork this Repository**: Fork this repository to your GitHub account.

2. **Add Secrets**: In your forked repository, go to "Settings" > "Secrets" and add the following secrets:
   - `SSH_PRIVATE_KEY`: Add the private SSH key that will be used to authenticate with the deployment server.
   - `VM_HOST`: Add the hostname or IP address of the deployment server.
   - `VM_USERNAME`: Add the username used to connect to the deployment server.

3. **Configure Deployment Server**: Ensure that your deployment server is properly configured to receive the deployed application. Adjust the deployment steps in the workflow according to your specific deployment requirements.

4. **Push to Repository**: Make changes to your Java code, commit, and push them to the `develop` or `main` branch. The CI/CD pipeline will be triggered automatically.


### Customize the Workflow

You can customize the workflow according to your project's needs. For example, you can add additional steps to run tests, perform static code analysis, or integrate with other tools.

### Workflow Diagram

![Workflow Diagram](http://www.plantuml.com/plantuml/png/ZP7FIiD04CRl-nH3JdhemPEtqjWW4X6aGI_ImxgPf4EtCylkB9Atjyr62eBWRRy_VFk3sIIrKVF9c_bXp6eDrVQ0xYXPOOT14gd4gPg33XLoWBPvXhlxOZrayZrOxk7LkgCTiTZRY5OHEhKZyGWDHNJNdRVWnVPGHwN1EgsCeG5kobINdSEKXknlGG_81c2rMdzCcFPGDHYyJD3APsNG9rn2bZrWZ19_U1ujUUpF7UvfC8L8UA0nHuIkUoeOZNpn3DBMkmLRmdHHe0BlByLR_gn3yEIial32Mu8Jilu3klGQOVoB_5hxZPzKWjcSnULEtm00)




## License

This project is licensed under the [MIT License](LICENSE). Feel free to use, modify, and distribute the code according to the terms of the license.

## Contributing

We welcome contributions from the community! If you'd like to contribute to this project, here's how you can get involved:

1. Fork this repository to your own GitHub account.
2. Create a new branch with a descriptive name for your contribution.
3. Make your changes, whether they're bug fixes, improvements, or new features.
4. Test your changes thoroughly.
5. Create a pull request (PR) from your branch to the `develop` or `main` branch of this repository.
6. Provide a clear description of your changes in the PR, along with any relevant context.

Our team will review your contribution and provide feedback. Thank you for helping us improve this project!

By contributing to this repository, you agree to abide by the [Code of Conduct](CODE_OF_CONDUCT.md) and the terms of the [MIT License](LICENSE).




