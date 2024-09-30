### **Git Workflow and Best Practices [BookingJini Labs]**

#### **1. Introduction**

This document outlines the Git workflow, best practices, and conventions that should be followed to ensure an organized, efficient, and scalable development process for our projects. It covers the structure for handling feature development, bug fixes, staging, and production deployments using industry standards.

---

#### **2. Branching Strategy**

##### **Main Branches**

In our project, we maintain three primary branches:

- **`main`**: The production branch that reflects the stable version of the application running in production.
- **`stage`**: The staging branch used to simulate the production environment for final testing before releasing to production.
- **`dev`**: The development branch where active development happens. All new features, enhancements, and bug fixes are first integrated here.

##### **Branch Naming Conventions**

We follow specific naming conventions to maintain clarity and consistency across our workflow:

- **Feature Branches**: `feature/<short-description>`
  - Created from: `dev`
  - Merged into: `dev`
  - Example: `feature/user-authentication`
  
- **Hotfix Branches**: `hotfix/<short-description>`
  - Created from: `main`
  - Merged into: `main`, `dev`, `stage`
  - Example: `hotfix/login-bug`
  
- **Bugfix Branches**: `bugfix/<short-description>`
  - Created from: `dev`
  - Merged into: `dev`
  - Example: `bugfix/fix-crash-on-login`

---

### **3. Development Workflow**

#### **Creating a Feature Branch**

When starting work on a new feature:

1. **Checkout the `dev` branch**: Always begin by updating your local `dev` branch with the latest changes:   
   ```bash
   git checkout dev
   git pull origin dev
   ```

2. **Create a new feature branch**: The branch name should follow the convention `feature/<short-description>`:   
   ```bash
   git checkout -b feature/user-authentication
   ```

---

#### **Commit Messages**

We use the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standard for writing commit messages to maintain consistency:

- **Structure**: `type(scope): subject`
  
- **Types**:
  - `feat`: New feature
  - `fix`: Bug fix
  - `docs`: Documentation changes
  - `style`: Code style changes (formatting, no logic change)
  - `refactor`: Code refactoring
  - `test`: Adding or updating tests
  - `chore`: Routine tasks, like updating build scripts

- **Message Guidelines**:
  - **Use the imperative mood**: Write as if you are giving a command (e.g., "Add feature" instead of "Added feature").
  - **Keep the subject line concise**: Aim for 50 characters or less for easy readability.
  - **Capitalize the first letter of the subject**: This enhances clarity and professionalism.
  - **Avoid punctuation at the end of the subject line**: No period is needed.
  - **Be specific but brief**: Clearly describe the change without unnecessary detail.

- **Example**:  
  ```bash
  git commit -m "feat(auth): add JWT-based authentication"
  ```

By adhering to these conventions, we ensure that our commit messages are clear, systematic, and helpful for both developers and automated tools at BookingJini.

--- 

#### **Pushing and Pull Requests**

After completing the feature:

1. **Push the branch to the remote repository**:   
   ```bash
   git push origin feature/user-authentication
   ```

2. **Create a Pull Request (PR)**:  
   - The PR should be from `feature/user-authentication` to `dev`.
   - Add a descriptive title and explanation of changes made in the PR description.
   - Assign **reviewers** for code review.
   - The **reviewer** is responsible for reviewing and merging the PR if it meets the required standards.

#### **Code Review and Feedback**

- **Reviewers** will check the code for functionality, bugs, and adherence to coding standards.
- If the PR is **approved**:
  - The **reviewer** will merge the feature branch into the `dev` branch.
- If the PR **requires changes**:
  - The reviewer will request changes via comments.
  - The **developer** will make the necessary changes, push them to the feature branch, and the PR will be updated automatically.

#### **Merging to `dev`**

When the PR is **approved**, the reviewer will merge the feature branch into `dev`. This ensures that no unreviewed code is merged into the main development branch.

**Branch Deletion:** After merging, the feature branch should be deleted to keep the repository clean. GitHub usually offers an option to delete the branch after merging.

---

### **4. Staging Workflow**

#### **Merging `dev` into `stage`**

When the `dev` branch is confirmed to be stable and ready for staging, the **Project Lead** will handle merging `dev` into `stage` using Pull Requests:

1. **Create a Pull Request (PR)**:
   - Open a PR from the `dev` branch into the `stage` branch.
   - This PR allows for review and discussion about the changes being merged.

2. **Review and Approval**:
   - The Project Lead or another team member will review the PR to ensure all changes are appropriate and stable.

3. **Merge the PR**:
   - Upon approval, the Project Lead or the designated reviewer will use the GitHub interface to merge the PR into the `stage` branch. This can include options to squash commits or merge without fast-forwarding as needed.

4. **Deployment**:
   - The `stage` branch will automatically be deployed to the staging environment based on the configured CI/CD pipeline.

#### **Handling Selective Feature Deployment with Cherry-Picking**

If you only want to deploy specific features from `dev` to `stage` (and not the entire branch), you can use Git’s **cherry-pick** feature. This allows you to bring in changes from individual commits without merging everything from `dev`.

1. **Identify the Commit Hash**:
   - Use `git log` to find the commit hash for the feature you want to bring into `stage`.
   ```bash
   git log
   ```

2. **Create a New Branch for the Cherry-Picked Commit**:
   - Create a new branch based on the `stage` branch.
   ```bash
   git checkout -b feature/specific-feature stage
   ```

3. **Cherry-Pick the Commit**:
   - Cherry-pick the specific commit (e.g., commit `abc1234`) from `dev`:
   ```bash
   git cherry-pick abc1234
   ```

4. **Create a Pull Request (PR)**:
   - Open a PR from the `feature/specific-feature` branch into the `stage` branch.

5. **Review and Approval**:
   - The Project Lead or another team member will review the PR.

6. **Merge the PR**:
   - Upon approval, merge the PR into the `stage` branch.

#### **Testing in Staging**

- **User Acceptance Testing (UAT)**:
   - Perform UAT and other relevant tests in the staging environment.
   - Work with QA teams to ensure the feature behaves as expected.

- **Bug Fixes**:
   - If issues are found, fix the bugs in `dev` and create a PR for the fix.
   - Retest in the staging environment before moving to production.

---

### **5. Production Workflow**

#### **Merging `stage` into `main`**

Once testing in the staging environment is successfully completed and all issues are resolved, the **Project Lead** will manage the merging process from `stage` to `main`:

1. **Create a Pull Request (PR)**:  
   - The Project Lead will create a PR to merge the `stage` branch into `main` to document the changes and facilitate a final review.

2. **Final Review and Approval**:  
   - The Project Lead will review the PR to ensure that all features and fixes are production-ready and that the `stage` branch has passed all necessary tests, including User Acceptance Testing (UAT).

3. **Merge the PR on GitHub**:  
   - After approval, the Project Lead will merge the PR directly on GitHub. This automatically combines the changes from `stage` into `main` without the need for local merging.
   
4. **Deployment**:  
   - The `main` branch is automatically deployed to the production environment as part of the CI/CD pipeline.
   - Monitor the production environment post-deployment to ensure everything is functioning correctly.

---

### **6. Hotfix Workflow**

##### **Creating a Hotfix Branch**

For urgent production issues (e.g., critical bugs):

1. **Create a `hotfix` branch from `main`**:  
   - Start by branching off the `main` branch to isolate the fix.
   ```bash
   git checkout -b hotfix/login-bug main
   ```

2. **Implement and test the fix**:  
   - Develop the required fix and perform **local testing** to ensure the issue is resolved without introducing new problems.

##### **Merging Hotfixes**

Once the hotfix has been tested:

1. **Create a Pull Request (PR)**:  
   - Open a PR from the `hotfix/login-bug` branch into the `main` branch. This PR will be reviewed by the Project Lead or Release Manager.

2. **Review and Approval**:  
   - The Project Lead or Release Manager will review the PR, ensuring that the hotfix is appropriate and has been tested adequately.

3. **Merge the PR**:  
   - Upon approval, the Project Lead or Release Manager will use the GitHub interface to merge the PR into the `main` branch. This can include options to squash commits or merge without fast-forwarding as needed.

4. **Merge back into `dev` and `stage`**:  
   - It’s crucial to ensure that the hotfix is included in ongoing development by merging it back into both the `dev` and `stage` branches. This prevents the fix from being overwritten by future merges.
   - Open PRs for the hotfix branch into both `dev` and `stage`, ensuring they are reviewed and approved.

5. **Delete the hotfix branch** (optional):  
   - Once the hotfix is merged into `main`, `dev`, and `stage`, you can safely delete the `hotfix` branch:
   ```bash
   git branch -d hotfix/login-bug
   ```

--- 


### **7. Branch Cleanup**

After merging a feature or hotfix branch into its target branch, it's important to perform branch cleanup to maintain a tidy repository:

1. **Delete the feature or hotfix branch locally**:  
   - After the branch has been successfully merged, delete it from your local environment:
   ```bash
   git branch -d feature/user-authentication
   git branch -d hotfix/login-bug
   ```

2. **Delete the branch from the remote repository**:  
   - Remove the branch from the remote repository to avoid clutter:
   ```bash
   git push origin --delete feature/user-authentication
   git push origin --delete hotfix/login-bug
   ```

### **Why Cleanup Matters**:
- **Clarity**: Deleting branches that have been merged reduces clutter and makes it easier to identify active branches in the repository.
- **Avoid Confusion**: It prevents team members from mistakenly working on outdated or obsolete branches.
- **Best Practices**: Keeping the repository organized aligns with industry standards for collaborative software development.

---

### **8. Best Practices**

#### **Commit Messages**
- Follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) format to ensure consistency and clarity in your commit history.
- Write clear, concise messages that accurately describe the changes made. A good commit message helps team members understand the purpose of changes without having to dig into the code.

#### **Code Reviews**
- Every Pull Request (PR) should be reviewed by at least one other developer before merging to ensure code quality and maintainability.
- Reviewers should focus on the following:
  - Adherence to coding standards and best practices.
  - Comprehensive testing and documentation.
  - Overall readability and maintainability of the code.

#### **Automated Testing and CI/CD**
- Implement automated testing for all code changes to catch issues early and improve software reliability.
- Use Continuous Integration/Continuous Deployment (CI/CD) pipelines to automatically:
  - Test code changes upon each commit or PR.
  - Deploy code to `dev`, `stage`, and `main` branches, reducing manual deployment errors and improving workflow efficiency.

#### **Branch Protection Rules**
- Protect the `main`, `stage`, and `dev` branches by enforcing PRs and code reviews before any merging to safeguard against unreviewed code entering these critical branches.
- Set up status checks in your CI pipeline to prevent merging code that fails tests. This ensures that only code that passes all required checks is integrated into these important branches.

### **Summary**
Adhering to these best practices fosters a collaborative, efficient, and high-quality development environment, ensuring that the codebase remains robust and maintainable.



### **Conclusion**

By following this detailed Git workflow, our development process will remain organized, efficient, and scalable. Adhering to these industry-standard practices ensures consistency, reduces errors, and streamlines our path from development to production.
