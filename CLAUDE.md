# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a KiCad library repository containing symbols, footprints, and 3D models for various electronic components that were needed but missing in the author's design work. The library is distributed as a KiCad Package Manager (PCM) addon.

## Repository Structure

- `symbols/` - KiCad symbol libraries (.kicad_sym files)
- `footprints/` - KiCad footprint libraries (.pretty directories with .kicad_mod files)  
- `3dmodels/` - 3D model files for components (.3dshapes directory)
- `resources/` - Additional resources like icons
- `package.py` - Python script for packaging releases
- `metadata.json` - PCM addon metadata with version history
- `metadata.template.json` - Template for generating metadata

## Common Commands

### Creating a Release
```bash
python package.py [version]
```
- Interactive version input if no argument provided
- Generates release zip in `build/` directory
- Updates `metadata.json` with new release info
- Version format: major[.minor[.patch]] (e.g., 1.0.3)

### Manual Release Process
1. Run `python package.py` with desired version
2. Create GitHub release with same version tag  
3. Upload generated zip file from `build/` directory
4. Fork KiCad addon metadata repository on GitLab
5. Submit metadata.json and icon.png via merge request

## KiCad Library Architecture

### File Organization
- **Symbol files**: `.kicad_sym` format containing electrical symbols
- **Footprint libraries**: `.pretty` directories containing `.kicad_mod` footprint files
- **3D models**: Stored in `.3dshapes` directory structure
- **Naming convention**: Uses `yvolodym` prefix for library organization

### Metadata Management
- PCM schema v1 compliant metadata structure
- Automatic version tracking with SHA256 checksums
- Download URLs point to GitHub releases
- License: CC-BY-SA-4.0

## Automation

### GitHub Actions (.github/workflows/release.yml)
- Triggers on GitHub release creation
- Automatically runs `package.py` with release tag
- Uploads built addon zip as release asset
- Commits updated `metadata.json` to main branch

## Development Guidelines

### Library Standards
- Follow KiCad library conventions for naming and organization
- Footprints designed per component datasheet land pattern recommendations
- Create components that fill gaps in existing libraries
- Maintain compatibility with KiCad 9.0.0+

### File Structure
- Keep symbols organized in single `.kicad_sym` file per library
- Group related footprints in dedicated `.pretty` directories
- Ensure 3D models are properly linked to footprints
- Maintain consistent naming across symbol/footprint/3D model triplets