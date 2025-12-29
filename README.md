# Node.js Microservice Template

A production-ready Node.js microservice template with Docker and GitHub CI/CD support.

## Features

- ✅ Express.js web framework
- ✅ Security middleware (Helmet, CORS)
- ✅ Request logging (Morgan)
- ✅ Health check endpoint
- ✅ Docker support with multi-stage builds
- ✅ GitHub Actions CI/CD pipeline
- ✅ Environment variable configuration
- ✅ Error handling middleware
- ✅ Non-root user in Docker container

## Quick Start

### Local Development

1. Install dependencies:
```bash
npm install
```

2. Copy environment file:
```bash
cp .env.example .env
```

3. Start the development server:
```bash
npm run dev
```

4. The server will be available at `http://localhost:3000`

### Docker

1. Build the Docker image:
```bash
docker build -t node-microservice-template .
```

2. Run the container:
```bash
docker run -p 3000:3000 --env-file .env node-microservice-template
```

### Testing

Run tests:
```bash
npm test
```

Run linter:
```bash
npm run lint
```

## API Endpoints

- `GET /` - Welcome message
- `GET /health` - Health check endpoint
- `GET /api` - API endpoint

## Project Structure

```
.
├── src/
│   ├── index.js          # Main application entry point
│   └── routes/           # Route handlers
├── .github/
│   └── workflows/
│       └── ci.yml        # GitHub CI workflow
├── Dockerfile            # Docker configuration
├── .dockerignore         # Docker ignore file
├── .env.example          # Environment variables example
├── package.json          # Dependencies and scripts
└── README.md            # This file
```

## Environment Variables

- `PORT` - Server port (default: 3000)
- `NODE_ENV` - Environment (development/production)
- `SERVICE_NAME` - Service name for health checks

## CI/CD

The GitHub Actions workflow includes:

### On Pull Requests:
- Run tests on multiple Node.js versions (18.x, 20.x)
- Run linter
- Build Docker image (without pushing)

### On Push to Main Branch:
- Run tests and linter
- Build and push Docker image to Docker Hub with auto-generated tag
- **Autopromotion**: Automatically update `deploy/kustomization.yaml` with the new image tag

### Required GitHub Secrets

For the CI/CD pipeline to work, you need to configure these secrets in your GitHub repository:

1. `DOCKER_USERNAME` - Your Docker Hub username
2. `DOCKER_PASSWORD` - Your Docker Hub password or access token

To set up secrets:
1. Go to your repository on GitHub
2. Navigate to Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Add `DOCKER_USERNAME` and `DOCKER_PASSWORD`

### Autopromotion Feature

When code is merged to the `main` branch:
1. A Docker image is built and tagged with format: `v1.0.{TIMESTAMP}-{SHORT_SHA}`
2. The image is pushed to Docker Hub
3. The `deploy/kustomization.yaml` file is automatically updated with the new image tag
4. The changes are committed back to the repository with `[skip ci]` to prevent infinite loops

## License

MIT

