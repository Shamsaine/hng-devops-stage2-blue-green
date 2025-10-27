# HNG DevOps Stage 2 - Blue-Green Deployment

A Blue-Green deployment model implementation for the HNG DevOps internship program.

## 📖 About

This repository demonstrates a **Blue-Green deployment strategy**, a modern deployment technique that minimizes downtime and reduces risk by running two identical production environments. Only one environment (Blue or Green) serves live production traffic at any given time, while the other remains idle or is used for testing new releases.

## 🎯 What is Blue-Green Deployment?

Blue-Green deployment is a release management strategy that involves:
- **Blue Environment**: The current production environment serving live traffic
- **Green Environment**: The new version of the application ready for deployment

When deploying a new version, traffic is switched from Blue to Green. If issues arise, traffic can be quickly rolled back to the Blue environment.

## ✨ Features

- Zero-downtime deployments
- Instant rollback capabilities
- Reduced deployment risk
- Easy testing of new releases in production-like environment

## 🚀 How It Works

1. The Blue environment runs the current production version
2. Deploy the new version to the Green environment
3. Test the Green environment thoroughly
4. Switch traffic from Blue to Green
5. Green becomes the new production environment
6. Blue environment is kept for quick rollback if needed

## 📋 Prerequisites

Before using this deployment model, ensure you have:
- Infrastructure capable of running two parallel environments
- Load balancer or traffic routing mechanism
- Monitoring and health check systems
- Automated deployment pipeline

## 🛠️ Getting Started

(Instructions will be added based on specific implementation)

## 🤝 Contributing

This project is part of the HNG DevOps internship program. Contributions and improvements are welcome!

## 📝 License

This project is part of the HNG Internship Stage 2 DevOps track.

---

**Author**: HNG Internship Program  
**Stage**: DevOps Stage 2
