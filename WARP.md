# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

VidTube is a complete YouTube clone backend built with Node.js, Express, and MongoDB. It implements a full-featured video hosting platform with user authentication, video upload/streaming, social features (likes, comments, subscriptions), and content management.

## Development Commands

### Basic Operations
- **Start development server**: `npm run dev` (uses nodemon for auto-reload)
- **Start production server**: `npm start`
- **Code formatting**: Use Prettier with configuration in `.prettierrc`

### Environment Setup
- Copy `.env.sample` to `.env` and configure required environment variables
- Required variables include MongoDB URI, Cloudinary credentials, JWT secrets, and CORS origin

## Architecture Overview

### Core Design Patterns
- **MVC Pattern**: Controllers handle business logic, Models define data schemas, Routes handle API endpoints
- **JWT Authentication**: Dual token system with access tokens (short-lived) and refresh tokens (long-lived)
- **Cloudinary Integration**: All media files (avatars, cover images, videos, thumbnails) are stored on Cloudinary
- **Mongoose Aggregation**: Extensive use of MongoDB aggregation pipelines for complex queries with pagination

### Authentication Flow
The app uses a sophisticated JWT-based auth system:
- Access tokens contain user details (id, email, username, fullName) and expire quickly
- Refresh tokens are stored in the database and used to generate new access tokens
- `verifyJWT` middleware protects routes by extracting tokens from cookies or Authorization header
- Most routes except `/register`, `/login`, and `/refresh-token` require authentication

### File Upload System
- Uses Multer middleware to handle multipart/form-data uploads
- Files are temporarily stored locally, then uploaded to Cloudinary
- Local files are automatically cleaned up after Cloudinary upload
- Support for multiple file types (videos, images) with field validation

### Database Schema Design
- **User**: Core entity with authentication, profile data, and watch history
- **Video**: Contains metadata, Cloudinary URLs, view counts, and ownership
- **Comment**: Nested comments system with video and owner references
- **Like**: Polymorphic likes system (videos, comments, tweets)
- **Subscription**: Channel subscription relationships
- **Tweet**: Social media functionality
- **Playlist**: Video collections with owner and video references

### API Structure
All API endpoints follow `/api/v1/{resource}` pattern:
- User management: registration, login, profile updates, channel info
- Video operations: upload, CRUD, publish/unpublish toggle
- Social features: likes, comments, subscriptions
- Content organization: playlists, dashboard analytics
- Micro-features: tweets, watch history

### Middleware Architecture
- **asyncHandler**: Wraps async route handlers to catch errors automatically
- **verifyJWT**: Validates JWT tokens and attaches user to request object
- **multer**: Handles file uploads with field-specific configurations
- **CORS**: Configured for credentials and specific origin

### Error Handling
- Custom `ApiError` class for consistent error responses
- `ApiResponse` class standardizes successful API responses
- Async error handling through wrapper functions

### Key Development Patterns
- Controllers use detailed comments outlining step-by-step logic
- Database operations often use `.select()` to exclude sensitive fields like passwords
- Extensive use of MongoDB aggregation for complex data relationships
- File operations always include cleanup logic for temporary files

### File Structure Logic
- `models/`: Mongoose schemas with pre-save hooks and instance methods
- `controllers/`: Business logic with comprehensive validation and error handling
- `routes/`: Express router definitions with middleware chaining
- `middlewares/`: Reusable middleware functions for auth and file handling
- `utils/`: Helper classes and functions for common operations
- `db/`: Database connection configuration

## Important Notes

- This is an educational project from a YouTube series on backend development
- The project emphasizes production-ready patterns like JWT refresh tokens, file cleanup, and comprehensive error handling
- All routes except authentication endpoints require JWT verification
- The codebase includes detailed comments explaining complex logic flows
- Uses ES6 modules throughout (type: "module" in package.json)

<citations>
<document>
<document_type>WARP_DOCUMENTATION</document_type>
<document_id>getting-started/quickstart-guide/coding-in-warp</document_id>
</document>
</citations>