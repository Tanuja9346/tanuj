1. What is MongoDB, and why is it used in DevOps?

MongoDB is a NoSQL database that stores data in JSON-like documents.
In DevOps, MongoDB is used because it supports high availability, replication, and scalability, making it ideal for cloud-based applications.


2. How do you install MongoDB on Linux?

sudo apt update
sudo apt install -y mongodb

3.  How do you check if the MongoDB service is running?
systemctl status mongod  # For systemd-based systems
service mongod status    # For older systems

4. How do you connect to MongoDB from the command line?
mongo and Or for newer versions: mongodbsh

5. How do you automate MongoDB backups?
mongodump --out /backup/mongodb-$(date +%F)

6. Use crontab to schedule backup?
0 2 * * * mongodump --out /backup/mongodb-$(date +%F)

7. What is MongoDB replication, and how do you set up a replica set?
Replication provides high availability by maintaining multiple copies of data.

Start MongoDB instances with --replSet option.
mongod --replSet "myReplicaSet" --port 27017 --dbpath /data/db1 --logpath /data/log1.log --fork

or Connect and initiate the replica set:
rs.initiate()
rs.add("mongo2:27018")
rs.add("mongo3:27019")


8. How do you monitor MongoDB performance?

mongostat
mongotop
Integrate MongoDB with Prometheus + Grafana for better visualization.

9. How do you scale MongoDB horizontally?
Use Sharding:
sh.enableSharding("myDatabase")
sh.shardCollection("myDatabase.myCollection", { "shardKey": 1 })

10. How do you secure MongoDB in a DevOps environment?

Enable authentication:
mongod --auth --keyFile /etc/mongodb-keyfile

Use AWS WAF & GuardDuty to monitor suspicious access.


Use firewall rules (e.g., allow only specific IPs):
ufw allow from 192.168.1.0/24 to any port 27017

11. Disable remote access if not needed:

bindIp: 127.0.0.1

12. How do you manage MongoDB logs efficiently?

rotate logs using logrotate:
sudo nano /etc/logrotate.d/mongodb
and 

/var/log/mongodb/*.log {
    daily
    rotate 7
    compress
    missingok
    notifempty
    copytruncate
}
13. How do you deploy MongoDB in Docker?

docker run -d --name mongo -p 27017:27017 -v /data/db:/data/db mongo

14. How do you deploy MongoDB in Kubernetes?
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo
        ports:
        - containerPort: 27017



