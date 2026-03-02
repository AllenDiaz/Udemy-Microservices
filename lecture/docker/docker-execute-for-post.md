# Lesson Summary: Dockerizing the Post Service

## Goal
Transform the **Post service** into a Docker image and run it inside a container, ensuring dependencies and startup commands are properly encapsulated.

---

## Steps to Dockerize

1. **Create a Dockerfile**
   - Must be named `Dockerfile` (capital D, no extension).
   - Commands included:
     - `FROM node:alpine` → Use Node Alpine as the base image.
     - `WORKDIR /app` → Set working directory.
     - `COPY package.json .` → Copy only `package.json`.
     - `RUN npm install` → Install dependencies.
     - `COPY . .` → Copy all remaining source code.
     - `CMD ["npm", "start"]` → Default command to start the service.

2. **Ignore Local Dependencies**
   - Create a `.dockerignore` file.
   - Add `node_modules` to prevent copying local dependencies into the image.
   - Ensures dependencies are installed fresh during image build.

3. **Build the Image**
   - Run in terminal:  
     ```bash
     docker build .
     ```
   - This pulls the base image, installs dependencies, copies files, and sets the default command.
   - Produces an **image ID** upon completion.

4. **Run a Container**
   - Use the image ID to start a container:  
     ```bash
     docker run <image_id>
     ```
   - The Post service runs successfully inside the container.

---

## Key Takeaways
- **Images** are blueprints; **containers** are running instances of those images.
- `.dockerignore` prevents unnecessary files (like `node_modules`) from bloating the image.
- Docker encapsulates dependencies and startup commands, making services portable and consistent across environments.

---

## Next Step
Review basic Docker commands to reinforce working with images and containers.
