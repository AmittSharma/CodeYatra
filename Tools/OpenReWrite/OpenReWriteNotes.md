🧠 What is OpenRewrite?
OpenRewrite is a static code analysis and transformation tool. It uses predefined recipes to automatically:

Upgrade Spring Boot versions

Update Java code for newer versions (Java 11/17/21)

Migrate deprecated APIs

Refactor configuration (like application.properties to application.yml)

Upgrade dependencies in pom.xml

It works with Maven and Gradle projects.

⚙️ Step 1: Prerequisites
✅ You need:

Java 17+ installed

Maven (mvn -v) or Gradle (gradle -v) installed

A Git-tracked project (optional but recommended)

🚀 Step 2: Initialize OpenRewrite in Your Maven Project
📁 Option 1: Use the OpenRewrite Maven Plugin
Add this to your pom.xml (at the bottom, inside <build><plugins>):

xml
Copy
Edit
<plugin>
<groupId>org.openrewrite.maven</groupId>
<artifactId>rewrite-maven-plugin</artifactId>
<version>5.27.0</version> <!-- Check for latest -->
<configuration>
<activeRecipes>
<recipe>org.openrewrite.java.migrate.UpgradeToJava17</recipe>
<recipe>org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_1</recipe>
</activeRecipes>
</configuration>
</plugin>
You can run it directly:

bash
Copy
Edit
mvn rewrite:run
🔄 Step 3: See What Changed
OpenRewrite makes direct changes to your source code and pom.xml.

You can inspect the changes using:

bash
Copy
Edit
git diff
📚 Step 4: Available Recipes
You can list all available recipes:

bash
Copy
Edit
mvn rewrite:discover
Some popular Spring + Java recipes:

Recipe ID	What it does
org.openrewrite.java.migrate.UpgradeToJava17	Refactor code to be Java 17 compatible
org.openrewrite.java.migrate.UseTextBlocks	Replace multiline strings with Java 15+ text blocks
org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_1	Migrate Spring Boot to 3.1
org.openrewrite.java.spring.boot2.UpgradeSpringBoot_2_7	Migrate Spring Boot to 2.7
org.openrewrite.java.dependencies.UpgradeDependencyVersion	Upgrade any library version
org.openrewrite.staticanalysis.CommonStaticAnalysis	Run general cleanup refactors

🛠️ Step 5: Customize Recipes
You can write your own YAML-based recipe for common upgrades:

Create a file: rewrite.yml

yaml
Copy
Edit
type: specs.openrewrite.org/v1beta/recipe
name: my.custom.recipe
displayName: My Custom Upgrade Recipe
recipeList:
- org.openrewrite.java.migrate.UpgradeToJava17
- org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_1
- org.openrewrite.java.cleanup.CommonStaticAnalysis
  Add this file in your project root.

Then update plugin config:

xml
Copy
Edit
<configuration>
<configLocation>rewrite.yml</configLocation>
<activeRecipes>
<recipe>my.custom.recipe</recipe>
</activeRecipes>
</configuration>
Then run:

bash
Copy
Edit
mvn rewrite:run
💡 Step 6: Gradle Projects
For Gradle, apply the plugin in build.gradle:

groovy
Copy
Edit
plugins {
id("org.openrewrite.rewrite") version("6.1.6")
}

rewrite {
activeRecipe("org.openrewrite.java.spring.boot3.UpgradeSpringBoot_3_1")
}
Then run:

bash
Copy
Edit
./gradlew rewriteRun
🧪 Step 7: Test Your Code
After running the recipe:

Build your project (mvn clean install)

Run your test cases

Fix any breaking changes due to deprecated APIs

📈 Step 8: Use in CI/CD (Optional)
You can run OpenRewrite as part of your CI/CD pipeline:

bash
Copy
Edit
mvn rewrite:run -Drewrite.configLocation=rewrite.yml
It can:

Upgrade dependencies in PRs

Fix legacy code in bulk

🛑 Gotchas & Tips
Issue	Solution
Project not Java 11+	Upgrade your JDK first
Rewrite not changing anything	Check if your recipe is compatible with your current versions
Need dry-run	Use git diff before committing
Too many changes	Run recipe-by-recipe incrementally

🏁 Final Workflow Summary
Backup or branch your project

Add rewrite plugin and recipes

Run mvn rewrite:run

Review changes using git diff

Fix test failures and deprecated usages

Commit and repeat for next module/project

💬 Want Help for Your Specific Project?
I can help you write a rewrite.yml file customized for your dependencies (e.g., Spring Boot 1.5 + Hibernate 5 → Spring Boot 3.1 + Hibernate 6). Just share:

Current Spring Boot version

Java version

Other important libraries (Kafka, DB, etc.)