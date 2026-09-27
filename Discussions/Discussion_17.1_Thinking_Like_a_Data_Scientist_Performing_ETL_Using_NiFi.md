# Self-Study Discussion 17.1: Thinking Like a Data Scientist - Performing ETL Using NiFi

One resource that helped me throughout Module 17 was the **Apache NiFi User Guide**.

## Resource

**Apache NiFi Documentation**

**Link:**  
https://nifi.apache.org/documentation/

## How It Helped

The Apache NiFi documentation was extremely helpful while completing the ETL activities and final assignment. The documentation provides detailed explanations of processors, controller services, FlowFiles, relationships, and processor properties. While building the Excel-to-CSV and CSV-to-MySQL pipelines, I frequently referenced the documentation to better understand how processors such as **GetFile**, **SplitText**, **ConvertRecord**, **ConvertJSONToSQL**, and **PutSQL** work together in an ETL workflow.

---

## Additional Resource

**Docker Hub**

**Link:**  
https://hub.docker.com/

## How It Helped

Docker Hub helped me better understand the NiFi and MySQL container images used throughout this module. The examples and documentation provided guidance on container deployment, networking, and troubleshooting. Since the assignment required communication between NiFi and MySQL through Docker containers, this resource was useful when verifying container connectivity and configuring the environment.

---

## Tip

One lesson I learned during the assignment is to carefully verify processor relationships and auto-termination settings in NiFi. Several validation errors occurred because processor relationships such as **failure**, **success**, or **retry** were neither connected to another processor nor configured for automatic termination. Hovering over the warning icons in NiFi provided valuable troubleshooting information and made it easier to identify and resolve configuration issues.

Another helpful practice is to configure and validate each processor individually before connecting the entire pipeline. This approach simplifies troubleshooting and reduces errors when building larger ETL workflows.

---

## Conclusion

I have bookmarked both the Apache NiFi Documentation and Docker Hub because they are valuable resources for data engineering projects. They helped me successfully complete the ETL assignments in this module and will continue to be useful references as I work with data pipelines, Docker environments, and data integration tools in future projects.