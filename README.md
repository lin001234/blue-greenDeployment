# Blue-Green deployment using docker and nginx

## How it works
Blue-Green deployment is used to ensure no downtime when pushing or changing new features.
To ensure this, without using k8s, I used 2 containers, blue and green containers, which are both running.
So lets say i finished changing and testing my new feature, which will be done in my green branch, when I push to the green branch, Github actions will run my ansible playbook, specifically the green_deploy.
As the current webapp would be running in blue container, playbook will take down the green containers, rebuild the updated green containers and update nginx to redirect to the green containers, with health checks. This results in no downtime when changing the features and allow for safe rollback as if healthcheck fails, it will redirect back to the previous blue container.

### Upsides
1. No downtime when changing new features
2. Automated by github actions and ansible
3. Can be changed to canary deployment by changing nginx configuration to allow weighing but it have to be done manually

### Downsides
1. Currently, if webapp is running on green container and i pushed to the green branch, it will result in downtime as my automation does not dynamically detect which container is actually running.
2. As I am using the git branch, the dockerfile is less efficient than when if i use the code in directory, due to it having to clone from github branch, resulting in not fully utilising the caching feature of docker.