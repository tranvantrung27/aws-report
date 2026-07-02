---
title: "Workshop"
date: 2026-04-26
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# IoT Weather Platform — Detailed Deployment Guide

#### Overview

The **IoT Weather Platform** is a real-time weather monitoring platform built on an AWS Serverless architecture for the *ITea Lab* team. The system collects data from weather stations using Raspberry Pi + ESP32 devices, processes and stores it via AWS services, and visualizes it through a web dashboard.

In this section, we walk through the complete system deployment steps end-to-end — from edge hardware configuration, AWS infrastructure setup, to the web interface and testing.

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Environment Setup](5.2-Prerequiste/)
3. [AWS IoT Core & S3 Data Lake Setup](5.3-S3-vpc/)
4. [Data Processing with AWS Glue](5.4-S3-onprem/)
5. [Web Dashboard Deployment with Amplify](5.5-Policy/)
6. [Resource Cleanup](5.6-Cleanup/)