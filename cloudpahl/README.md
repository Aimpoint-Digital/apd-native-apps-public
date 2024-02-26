# Welcome to CloudPAHL by Aimpoint Digital! 

## INTRODUCTION
**The purpose of this application is to allow administrators & super users to explore, understand, and optimize their Snowflake account usage.**

**CloudPAHL** is comprised of four pages: 
1. **Platform Overview:** Summarizes both utilized and underutilized Snowflake Services 
2. **Audit Review:** Examines user behavior, identifies groups in need to additional training, and assesses for necessary action reviews or permission adjustments 
3. **Health Check Analysis:** Assesses cost and credit trends, attributing them to determine if Snowflake is scaling as anticipated or if unexpected costs are emerging and where 
4. **Lineage Visualization:** Displays role and object permissions 

## HOW TO
### (1) Set up application and necessary permissions
The application requires access to the **SNOWFLAKE** database. Please run these commands in a worksheet using the **ACCOUNTADMIN** role to grant the application access to this database and setup the app. You can choose the cadence at which you'd like the underlying data to be refreshed. The standard options are 'daily', 'weekly', and 'monthly', or you can set a custom CRON schedule (with timezone). 
- 'daily' loads account usage data, creates task to run daily on the chosen warehouse at 4am UTC
- 'weekly' creates task to run weekly on the chosen warehouse, every Sunday at 4am UTC
- 'monthly' creates task to run monthly on the chosen warehouse, on the 1st of every month at 4am UTC

The refresh job is run as a task which requires a warehouse to be declared as well. You can also choose *NOT* to set a regular cadence (leave the <refresh-cadence> and <warehouse> parameters empty), and re-run the `SETUP_CLOUDPAHL()` command to reload data as needed.

```
-- Grant privileges to app
GRANT EXECUTE TASK ON ACCOUNT TO APPLICATION CLOUDPAHL;
GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO APPLICATION CLOUDPAHL;

-- If you are scheduling a refresh task, input the warehouse you want the task to use
GRANT USAGE ON WAREHOUSE <warehouse> TO APPLICATION CLOUDPAHL; 

-- Input desired task cadence and the warehouse that you granted usage to above
CALL CLOUDPAHL.CACHE_TABLES.SETUP_CLOUDPAHL(<refresh-cadence>, <warehouse>);

-- To reset or remove the task schedule
CALL CLOUDPAHL.TASK_SCHEMA.SETUP_TASK(); -- remove existing task
CALL CLOUDPAHL.TASK_SCHEMA.SETUP_TASK('*/3 * * * * Etc/UTC', 'compute_wh'); -- update task schedule
```
#### Example inputs
```
CALL CLOUDPAHL.CACHE_TABLES.SETUP_CLOUDPAHL();
-- loads account usage data once, does not create a recurring task

CALL CLOUDPAHL.CACHE_TABLES.SETUP_CLOUDPAHL('daily', 'compute_wh');

CALL CLOUDPAHL.CACHE_TABLES.SETUP_CLOUDPAHL('0 0,12 * * * Etc/UTC', 'compute_wh');
-- loads account usage data, creates task to run twice a day on the compute_wh warehouse, at midnight and noon UTC
```

*The time needed for the setup process to complete can vary with the size of your account. When you see this, or a similar result from `SETUP_CLOUDPAHL()`, you can continue on to the next steps:*
```Setup complete: Data loaded. Task has been created using a cadence of daily```

### (2) Navigate to Landing Page and Load Data
Once the setup process is complete, please navigate to the **'CloudPAHL App Interface'** tab located in the top-left of your screen. 
Click the blue **'Load Data'** button to access the data from Setup. 

![Click the blue "Load Data" button to access the data from Setup](screenshots/image.png)

Confirm data has been loaded

![Confirm data has been loaded](screenshots/image-1.png)

### (3) Explore App
Click through the pages in the sidebar. 

You'll see this progress bar as the page loads. 

![You'll see this progress bar as the page loads](screenshots/image-2.png)

When the page is fully loaded the status bar will turn green

![When the page is fully loaded the status bar will turn green](screenshots/image-3.png)

### (4) Change Date Filters
Choose a new date range. 

![Date Filters](screenshots/image-4.png)

Click **'Apply'** to submit selected dates. 

![Apply new filter range](screenshots/image-5.png)

The page will reload with data from selected date range

## ABOUT & HELP
This application was developed by **[Aimpoint Digital](https://aimpointdigital.com/)** - an end-to-end analytics firm that uses data to solve its clients’ most complex use-cases. We offer consulting support across data engineering, data analytics, data science and analytic strategy practices. As an **Elite Snowflake Partner** we can help you with anything from migrations, to native apps to advanced ML in Snowflake.

If you encounter any issues, please do reach out to us directly at **nativeapps@aimpointdigital.com** - be sure to include as many screenshots and detail as possible.