# 3D Files Importer

The 3D Files Importer is a backend service that transforms 3D model files into a standardized format suitable for the HSML stantard defined by the **Spatial Web Foundation** in collaboration with the **IEEE**. 

It processes various 3D file formats and converts them into deployable 3D content that can be used within the digital twin ecosystem.

## What It Does

### Core Functionality

The 3D Files Importer takes 3D model files (commonly in .blend format, but supporting multiple formats) and converts them into platform-ready assets:

- **File Processing**: Accepts 3D model files through an API interface and processes them for platform compatibility
- **Format Conversion**: Transforms input 3D files into multiple optimized mesh representations suitable for different target platforms
- **Multiplatform Interoperability**: Generates multiple asset formats to support various 3D engines and platforms simultaneously, enabling the same entity to be represented differently across different rendering environments (e.g., Unity, NVIDIA Omniverse)
- **Domain Creation**: Generates HSML (Hierarchical Space Modeling Language) Domain entities in Genius Core - the platform's data representation format
- **Model Generation**: Creates corresponding HSML Model entities that represent the 3D geometry, materials, and metadata
- **Collision Mesh Generation**: Uses pybullet to generate V-HACD (Voxelized Hierarchical Approximate Convex Decomposition) meshes for accurate collision detection in physics-enabled applications
- **Asset Upload**: Uploads all processed 3D meshes to cloud storage (S3) for delivery to end-user applications

### Processing Pipeline

When a user submits a 3D file through the API:

1. The file is received and validated
2. The 3D geometry is extracted and processed
3. Multiple output formats are generated for different platforms (web, Unity, Omniverse, etc.)
4. Collision meshes are computed using physics-based decomposition
5. Mesh optimization is applied to each output format (optional, configurable)
6. HSML Domain and Models are created in Genius Core
7. All assets are uploaded to S3 for cloud storage and management
8. Results are returned to the caller

### Key Features

- **Multi-Format Output**: Generates optimized assets for multiple platforms simultaneously, ensuring interoperability across different 3D engines
- **REST API Interface**: Easy integration with other services and workflows
- **Async Processing**: Uses Celery for background task processing, handling large files without blocking
- **Collision Detection**: Generates V-HACD collision meshes using physics simulation for accurate physics interactions
- **Mesh Optimization**: Optionally optimizes 3D meshes for better performance in web and mobile contexts
- **Flexible Deployment**: Can run locally for development/testing or deployed with cloud storage integration
- **Multiple Format Support**: Accepts various 3D file formats
- **S3 Asset Management**: All generated meshes are stored and managed via AWS S3 for scalable asset delivery

## Architecture

The service is designed as a Python-based microservice that sits between 3D content creators/editors and the Genius Core platform:

- **API Layer**: FastAPI-based REST interface for file submission and status checking
- **Processing Layer**: Core business logic for 3D file conversion, multi-format export, collision mesh generation, and HSML generation
- **Storage Layer**: Integration with S3 for asset storage and retrieval
- **Integration Layer**: Communication with Genius Core via HSTP protocol

### Configuration

The service uses environment-based configuration, allowing easy deployment across different environments (development, staging, production).

Supported configuration options include:
- Genius Core endpoint URL
- AWS S3 settings for asset storage
- Processing options (mesh optimization toggle)
- Local-only mode for development without cloud dependencies

## Use Cases

The 3D Files Importer is used in workflows such as:

- **Content Import**: Importing 3D assets created in tools like Blender into the Verses platform
- **Digital Twin Creation**: Building 3D representations of physical spaces for digital twin applications
- **Multiplatform Asset Preparation**: Converting 3D content into optimized formats for different rendering engines simultaneously
- **Physics-Enabled Applications**: Generating collision meshes for simulation and physics-based interactions
- **Asset Management**: Converting and storing 3D content for delivery to various applications
- **Batch Processing**: Processing multiple 3D files through automation scripts

## Getting Started

1. Configure environment variables (template provided)
2. Start the API service
3. Start the Celery worker for background processing
4. Submit 3D files via the REST API
5. Retrieve results and use the generated HSML entities in Genius Core

## API Documentation

The service exposes a REST API with endpoints for:
- Submitting 3D files for processing
- Checking processing status
- Retrieving generated HSML entities

Full API documentation is available at the `/apidocs` endpoint when the service is running.

## Testing

Integration tests are provided to verify API functionality and processing correctness.

---

The 3D Files Importer bridges the gap between 3D content creation tools and the Verses platform, enabling seamless integration of 3D assets into digital twin and spatial computing applications. Its multi-format output capability ensures that the same 3D entity can be effectively rendered across different platforms and engines, supporting the platform's commitment to interoperability.
