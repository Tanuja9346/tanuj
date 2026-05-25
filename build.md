dotnet restore	    Restores NuGet packages
dotnet build	    Compiles the project
dotnet test	        Runs unit tests
dotnet run	        Builds and runs the app
dotnet publish	    Prepares app for deployment (compiled + deps)
config file: .csproj, .sln

nodejs: npm install
config file: psackage.json

Language | Build Tool | Config File(s) | Common Build Command

Java | Maven | pom.xml | mvn clean install

Node.js | npm / yarn | package.json | npm install && npm run build

Python | pip / setuptools/poetry| requirements.txt, setup.py , pyproject.toml| pip install -r requirements.txt,python setup.py install,poetry install

.NET | dotnet CLI | .csproj, global.json, .sln| dotnet build

3. maven life cycle?

Lifecycle | Purpose

default | Handles your project build
clean | Cleans the project (e.g., deletes target/)
site | Generates project documentation

Phase | What It Does
validate | Checks if the project is correct and all necessary info is available
compile | Compiles source code
test | Runs unit tests (uses Surefire plugin)
package | Packages code into a .jar, .war, etc.
verify | Runs checks to verify the package is valid (e.g., integration tests)
install | Installs the package into local Maven repo (~/.m2)
deploy | Deploys the built artifact to a remote repository (like Nexus, Artifactory)

mvn clean install
clean → deletes target/

install triggers the full chain:

validate → compile → test → package → verify → install

