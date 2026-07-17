# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-07-17

### Added
- Comprehensive installation guide (INSTALLATION.md)
- Tap testing procedure guide (TESTING_GUIDE.md)
- Requirements.txt for simplified dependency management
- `.gitignore` file for proper version control
- Better documentation structure and user guidance
- Hardware assembly and wiring instructions
- Troubleshooting section in INSTALLATION.md
- Sample data interpretation guidance

### Improved
- Better code organization and comments
- Enhanced README with quick-start section
- Real hardware photos in documentation
- Actual data example from tap_20260622_163222.csv
- User-friendly error messages
- Setup procedure clarity

### Changed
- Moved from manual pip install commands to requirements.txt
- Reorganized documentation for better navigation
- Improved FFT analysis visualization

### Fixed
- Removed virtual environment folder from repository
- Corrected wiring diagram clarity
- Improved I2C configuration documentation

---

## [1.0.0] - 2026-07-15

### Added
- Initial working prototype
- FFT spectrum analysis with windowing
- Logarithmic decrement damping estimation
- Half-power bandwidth damping calculation
- Automatic dominant frequency detection
- Three-axis accelerometer support (X, Y, Z)
- CSV data export
- PNG spectrum plot generation
- RPM avoidance chart generation
- Automatic report text file creation
- Chatter axis selection algorithm
- Support for configurable number of tool flutes
- Headless operation capability
- Raspberry Pi 4B platform support

### Features
- Real-time vibration capture using MPU-6050
- Signal windowing (Hanning window)
- DC offset removal
- Single-sided FFT analysis
- Peak detection and filtering
- Bandwidth-based peak identification
- Temperature compensation for accelerometer
- Multiple damping estimation methods
- Flexible mode identification

### Tested On
- Raspberry Pi OS Bullseye
- Python 3.9+
- MPU-6050 I2C accelerometer

### Known Limitations
- Single-axis tap testing (no multi-axis FRF)
- No full Stability Lobe Diagram (simplified avoidance zones)
- Limited to harmonic analysis (no phase information)
- No instrumented hammer support
- No cutting force integration
- Limited to one accelerometer per system

---

## Future Roadmap

### [1.2.0] - Planned
- [ ] Web dashboard interface
- [ ] Database for storing tool profiles
- [ ] Mobile app integration
- [ ] Multiple accelerometer support (simultaneous 3D FRF)
- [ ] Instrumented hammer support
- [ ] Temperature compensation UI
- [ ] Data export to multiple formats (JSON, Excel)

### [2.0.0] - Long-term Vision
- [ ] Full Stability Lobe Diagram generation
- [ ] Frequency Response Function (FRF) analysis
- [ ] Cutting force integration
- [ ] Machine learning chatter detection
- [ ] Real-time spindle speed recommendations
- [ ] Tool library database
- [ ] Integration with CNC control software
- [ ] Wireless sensor network support
- [ ] AI-assisted troubleshooting

---

## How to Report Issues

Found a bug? Have a suggestion?

Please open an issue on [GitHub](https://github.com/rzega02/PiModal-CNC---Raspberry-Pi-CNC-Tap-Tester/issues) with:

1. **Title:** Brief description of the issue
2. **Description:** What you expected vs. what happened
3. **Steps to reproduce:** How to recreate the problem
4. **Environment:** Raspberry Pi model, OS version, Python version
5. **Screenshots/logs:** If applicable

---

## Versioning

This project uses [Semantic Versioning](https://semver.org/):

- **MAJOR** (X.0.0): Breaking changes to the API or user interface
- **MINOR** (1.X.0): New features, backwards compatible
- **PATCH** (1.0.X): Bug fixes, no new features

---

## Contributors

- **Robert Zega** - Project creator, Miyama USA, Inc.

Want to contribute? See [CONTRIBUTING.md](CONTRIBUTING.md) (coming soon!)

---

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.
