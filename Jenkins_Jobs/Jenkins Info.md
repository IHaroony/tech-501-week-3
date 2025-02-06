
### CI (Continuous Integration)

- **What is CI?**  
    CI is a practice where developers frequently merge their code changes into a central repository, which triggers automated builds and tests to detect integration issues early.
    
- **Benefits of CI**
    
    - Early detection of bugs
    - Faster release cycles
    - Improved code quality

### CD (Continuous Delivery)

- **What is CD?**  
    CD is an extension of CI, ensuring that code changes are automatically tested, built, and ready for release to production at any time, though manual approval is required for deployment.
    
- **Benefits of CD**
    
    - Faster time to market
    - Reduced manual deployment efforts
    - Lower risk of bugs in production

### Difference Between CD and CDE

- **Continuous Deployment (CDE)** is the next level of CD. While CD requires manual approval before deployment, CDE automatically deploys code to production as soon as it passes automated tests.

### Jenkins

- **What is Jenkins?**  
    Jenkins is an open-source automation server used to automate tasks like building, testing, and deploying software.
    
- **Why use Jenkins? Benefits of using Jenkins?**
    
    - Automates repetitive tasks
    - Integrates with many tools and plugins
    - Scalability and flexibility
- **Disadvantages of Jenkins?**
    
    - Can become complex with large-scale setups
    - Requires regular maintenance and updates

### Stages of Jenkins

1. **Source Code Management** (e.g., Git)
2. **Build** (compiling, packaging, etc.)
3. **Test** (running automated tests)
4. **Deploy** (deploying to staging or production)

### Alternatives to Jenkins

- GitLab CI/CD
- CircleCI
- Travis CI
- Bamboo

### Why Build a Pipeline? Business Value

- Pipelines automate the software delivery process, reducing human error, improving deployment speed, and ensuring consistent, reliable releases.