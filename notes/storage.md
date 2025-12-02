# Storage

---

## EBS Volume (Elastic Block Store)
- A network drive you can attach to your instances while they run.
- Allows your instances to persist data, even after their termination.
- Bound to a specific *availability zone*.
- *Like a network usb stick*.

---

## AMI (Amazon Machine Image)
- Customization of an EC2 instance.
- Add your own software, config, OS, and monitoring.
- Faster boot/config time because all your sofware is pre-packaged
- Built for a *specific region*.
- Can be copied across regions.
- **Can be launched from:**
    - **Public AMI** - AWS provided
    - **Your own AMI** - Make and maintain them yourself
    - **AWS Marketplace AMI** - AMI someone else makes (and potentially sells) 

---

## Amazon EFS (Elastic File System)
- Managed network file stystem that can be mounted on many EC2 instances.
- Works with EC2 instances in *Multi Availability zones*.
- *Highly Available + Scalable + Expensive*.