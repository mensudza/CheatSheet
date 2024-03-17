# Linux Workshop

<p>This section is a review of three labs about managing system with Linux</p>
<chapter title="Workshop#1: Manage User Account and Password" id="csc325-workshop1">
    <procedure title="" id="procedure-id">
       <p> 
                <control>Instruction:</control> From a given Linux host, each student must 
                manage user accounts and passwords by using the commands <shortcut>useradd</shortcut>, 
                <shortcut>groupadd</shortcut>, <shortcut>usermod</shortcut>, <shortcut>groupmod</shortcut>, 
                <shortcut>id</shortcut>, <shortcut>su - [username]</shortcut>, <shortcut>passwd</shortcut> and 
                <shortcut>chage</shortcut> as the following criteria:
        </p>
       <step>
        <p>Create a user "student01" including its home directory.</p>
        <code-block>useradd student01</code-block>
        </step>
        <step>
            <p>Create a group "student" with GID = 10000.</p>
            <code-block>groupadd student –g 10000</code-block>
        </step>
        <step>
            <p>Use the groupmod command to add student01 into the supplementary group “student”.</p>
            <code-block>usermod –G student student01</code-block>
        </step>
        <step>
            <p>Set the password of student01 as “Csc489Sit”. </p>
            <code-block>passwd student01</code-block>
        </step>
        <step>
            <p>Use "tail [filename]" command to show the last 10 lines of files: passwd, group, and shadow under the "/etc" directory.</p>
            <p>Consult the “man” command for the syntax and semantic of files: passwd and group. <emphasis>Explain the results related to the user student01</emphasis>.</p>
            <code-block>
                tail –n 10 /etc/passwd&nbsp;
                tail –n 10 /etc/group&nbsp;
                tail –n 10 /etc/shadow
            </code-block>
            <tip>The results show that UID and GID of student01 are 500, student01 is added to student group which GID is 10000, and encrypted password of student01.</tip>
        </step>
        <step>
            <p>Create another user “student02” including its home directory.</p>
            <code-block>useradd student02</code-block>
        </step>
        <step>
            <p>Change the UID and GID of student02 to 505 and add the student02 user into the supplementary group "student".</p>
            <code-block>
            usermod –u 505 student02
            groupmod –g 505 student02
            usermod –G student student02
            </code-block>
        </step>
        <step>
            <p>Show the UID, GID, and supplementary groups of the current user.</p>
            <code-block>
            id student02
            </code-block>
        </step>
        <step>
            <p>Change the current user from root to student02.</p>
            <code-block>su – student02</code-block>
        </step>
        <step>
            <p>Show the UID, GID, and supplementary groups of the login user again.</p>
            <code-block>
            id
            </code-block>
        </step>
        <step>
            <p>Try to set the password of student02 as “student02”. <emphasis>What is the password restriction?</emphasis></p>
            <warning>passwd: Authentication token manipulation error.</warning>
        </step>
        <step>
            <p>Set the password subjected to the restriction.</p>
            <code-block>passwd student02</code-block>
        </step>
        <step>
            <p>Show the last 10 lines in the /etc/{passwd,group,shadow} files to confirm the result.</p>
            <code-block>
            tail –n 10 /etc/passwd
            tail –n 10 /etc/group 
            tail –n 10 /etc/shadow 
            </code-block>
            <p>Go back to the root login again uses the exit command.</p>
        </step>
        <step>
            <p>Consult the man command for the syntax and semantic of the “shadow” file. What is the hash algorithm using to encrypt the passwords? What is the salt value of student01?</p>
            <tip>
            The hash algorithm using to encrypt the passwords is $6 which is SHA-512 Algorithm.
            <emphasis>Salt value of student01: H5h2GGHW</emphasis>
            </tip>
        </step>
        <step>
            <p>
            Use the usermod command to lock the student01 account.
            </p>
            <code-block>
            usermod –L student01 
            </code-block>
        </step>
        <step>
            <p>For the “student02” account, set the password policies as follows:</p>
            <list type="alpha-lower">
                <li>
                    <p>This account will be expired on Dec 31, 2016.</p>
                    <code>usermod -e 2016-12-31 student02</code> or <code>chage –E 2016-12-31 student02</code> 
                </li>
                <li>
                    <p>
                    The new password must be required every 30 days.
                    </p>
                    <code-block>
                    chage –M 30 student02
                    chage –m 0 student02
                    </code-block>
                </li>
                <li>
                    <p>User will be forced to change the password on the next login.</p>
                    <code-block>chage –d 0 student02</code-block>
                </li>
                <li>
                    <p>How many day the account remain active after the expiry date?</p>
                    <tip>The account will not active after the expiry date.</tip>
                </li>
            </list>
        </step>
    </procedure>
</chapter>
<chapter title="Workshop#2: Manage A Share Directory" id="csc325-workshop2">
    <procedure>
         <p><control>Instruction:</control> From a given Linux host, each student must 
            manage a share directory "/share/case_x" for a detective company by using the commands <shortcut>chown</shortcut>,<shortcut>chmod</shortcut>, <shortcut>setfacl</shortcut>, and <shortcut>getfacl</shortcut> as the following criteria:</p>
        <step>
            <p>
            Create users: mentor1, mentor2, trainee1, trainee2, and trainee3; then initial the password “centos” for all users.
            </p>
            <code-block>
            useradd mentor1
            passwd mentor1
            </code-block>
            <code-block>
            useradd mentor2
            passwd mentor2
            </code-block>
            <code-block>
            useradd trainee1
            passwd trainee1
            </code-block>
            <code-block>
            useradd trainee2
            passwd trainee2
            </code-block>
            <code-block>
            useradd trainee3
            passwd trainee3
            </code-block>
        </step>
        <step>
            <p>Create group and members as follows:</p>
            <list type="alpha-lower">
                <li>
                    <p>mentors group: mentor1, mentor2</p>
                    <code-block>
                    groupadd mentors
                    usermod -G mentors mentor1
                    usermod -G mentors mentor2
                    </code-block>
                </li>
                <li>
                    <p>trainees group: trainee1, trainee2, trainee3</p>
                    <code-block>
                    groupadd trainees
                    usermod -G trainees trainee1
                    usermod -G trainees trainee2
                    usermod -G trainees trainee3
                    </code-block>
                </li>
            </list>
        </step>
        <step>
            <p>Create a share directory “/share/case_x”.</p>
            <code-block>
            mkdir /share
            mkdir /share/case_x
            chmod 777 /share/case_x
            </code-block>
        </step>
        <step>
        <p>Using vi text editor to create a text file “text1” and a script file “hello1” in the directory case_x as follows:</p>
        <list type="alpha-lower">
            <li>
                <p>Text file “text1”
                Scotlandyard
                Holme
                Bakerstreet
                Watson 
                </p>
                <code-block>
                cd /share/case_x
                vi text1
                </code-block>
            </li>
            <li>
                <p>
                Script file “hello1”
                #!/bin/bash
                clear
                echo "Good morning, world."
                </p>
                <code-block>
                vi hello1
                chmod 777 hello1
                </code-block>
            </li>
        </list>
        </step>
        <step>
            <p>Recursively change group ownership of the case_x directory to group mentors.</p>
            <code-block>
            cd /share
            chown :mentors ./case_x/
            chown -R :mentors ./case_x/
            </code-block>
        </step>
        <step>
            <p>Allow read/write for the owner and group of the existing file under the case_x directory.</p>
            <code-block>
            chmod -R ug+rw ./case_x/
            </code-block>
        </step>
        <step>
            <p>Add execute permission for the group of the hello1 file under the case_x directory.</p>
            <code-block>
            cd /share/case_x/
            chmod 674 hello1
            </code-block>
        </step>
        <step>
            <p>
            Use permission setgid to enforce new files added in the case_x directory automatically belong to group mentors. 
            </p>
            <code-block>
            chmod g+s /share/case_x 
            </code-block>
        </step>
        <step>
            <p>Add ACLs to the case_x directory that allow the trainees group to have read/write on the files and read/write/execute directories.</p>
            <code-block>
            setfacl -m g:trainees:rwx /share/case_x
            setfacl -d -m g:trainees:rw /share/case_x
            </code-block>
        </step>
        <step>
            <p>
            Add ACL to the case_x directory that restricts the user trainee2 to read on the files and read/execute on the directories. 
            </p>
            <code-block>
            setfacl -m u:trainee2:rx /share/case_x
            setfacl -d -m u:trainee2:r /share/case_x
            </code-block>
        </step>
        <step>
            <p>Add ACL to ensure that any new files or directories added in the case_x directory have the permission complied to the ACLs from (8) and (9) for all authorized9 users and groups.</p>
        </step>
        <step>
            <p>User the commands ls and getfacl to verify your configures</p>
        </step>
        <step>
            <p>Switch to mentor1, trainee1, and trainee2 for testing the following operations:</p>
            <code-block>
            sudo –I –u [username]
            cd /share/case_x
            cat text1
            ./hello1
            echo “hello” > [username].txt
            mkdir [username].dir
            echo hello > [username].dir/test.txt
            ls –ld [username].dir
            ls –l [username].dir
            </code-block>
        </step>
        <step>
            <p>Capture the screen from (13).</p>
        </step>
    </procedure>
</chapter>
<chapter title="Workshop#3: Web Service" id="csc325-workshop3">
    <procedure>
        <p> 
            <control>Instruction:</control> From a given Linux host, each student must 
            nable web service on your host and change the default document director “/var/ls -www/html” 
            for the Apache server to “mywww” directory. The web document must be protected by two access controls: 
            file permission and SELinux. Using the commands <shortcut>chmod</shortcut>, 
            <shortcut>ls -Z</shortcut>, <shortcut>getenforce</shortcut>, <shortcut>setenforce</shortcut>, 
            <shortcut>semanage</shortcut>, and <shortcut>restorese</shortcut> to achieve the following procedures:
        </p>
        <step>
            <p>
            Create users: web1, web2; and then initial the password “apache” for all users. 
            </p>
            <code-block>
            useradd web1
            passwd web1
            useradd web2
            passwd web2
            </code-block>
        </step>
        <step>
            <p>Create group “webs” consists of “web1” and “web2” as members.</p>
            <code-block>
            groupadd webs
            usermod -G webs web1
            usermod -G webs web2
            </code-block>
        </step>
        <step>
            <p>Install web service by using the following steps:</p>
            <list type="alpha-lower">
                <li>
                    <p>Install Apache package</p>
                    <code-block>dnf install httpd</code-block>
                </li>
                <li>
                    <p>Using <code>vi</code> editor to add your host entry “[IP address] [your host name]” in "/etc/hosts" file.</p>
                    <code-block>vi /etc/hosts</code-block>
                </li>
                <li>
                    <p>Start HTTP service up </p>
                    <code-block>systemctl start httpd</code-block>
                </li>
                <li>
                    <p>
                    Automatically start the HTTP service at the boot time
                    </p>
                    <code-block>systemctl enable httpd</code-block>
                </li>
                <li>
                    <p>Allow TCP port 80 for HTTP on the firewall</p>
                    <code-block>
                    firewall-cmd –permanent –add-service=http 
                    firewall-cmd --reload
                    </code-block>
                </li>
            </list>
        </step>
        <step>
        To test the web service, open http://[your host IP address] on a browser. 
        </step>
        <step>
            <p>Create a directory “/mywww” for the new root directory of HTTP document.</p>
            <code-block>mkdir /mywww</code-block>
        </step>
        <step>
            <p>Use vi text editor to create a html file “index.html” in the root directory as follows:</p>
            <code-block>
            //html code
            </code-block>
        </step>
        <step>
            <p>To configure Apache to use the new root directory, open the “/etc/httpd/conf/httpd.conf” file and change the value on two directives “DocumentRoot” and “Directory” from "/var/www/html" to “/mywww”. </p>
            <code-block>
            vi /etc/httpd/conf/httpd.conf 
            </code-block>
        </step>
        <step>
            <p>Restart HTTP service with the command:</p>
            <code-block>systemctl restart httpd</code-block>
        </step>
        <step>
            <p>Based on the least privilege strategy, set the file permission “rwxr-x--x” for “/mywww” and “rwxr-xr-x” for all file under it.</p>
            <code-block>
            chmod 751 /mywww
            chmod 755 /mywww/* 
            </code-block>
        </step>
        <step>
            <p>Open http://[your host IP address] on a browser again. Do you see your html page? Why? </p>
            <tip>No, because only file permission was edited, but there also protected by SELinux </tip>
        </step>
        <step>
            <p>
            Use the ls command to display the file permission and SELinux label on the directory mywww and file index.html. 
            </p>
            <code-block>ls -Z</code-block>
        </step>
        <step>
            <p>Capture the result from (11) and explain the SELinux context from the capture.</p>
            <tip>SELinux context shows that the user is configured. The role is an object. The type is a default content in users home, and the level is 0.</tip>
        </step>
        <step>With the command getenforce, what is the current status of SELinux?</step>
        <step>With the command setenforce, set the SELinux status to “permissive”.</step>
        <step>
            <p>Open http://[your host IP address] on a browser again. Do you see your html page? Why? </p>
            <tip>Yes, because both file permission and SELinux were configured.</tip>
        </step>
        <step>
            <p>With the command setenforce, set the SELinux status to “enforcing”.</p>
        </step>
        <step>
            <p>Install SELinux policy management tool with the command:</p>
            <code-block>dnf install policycoreutils-python-utils”</code-block>
        </step>
        <step>
            <p>Add a SELinux file context rule that set the context type to “httpd_sys_content_t” for the directory mywww and all file below it with the command: </p>
            <code-block>semanage fcontext -a -t httpd_sys_content_t ‘/mywww(/.*)?’</code-block>
        </step>
        <step>
            <p>Relabel directors mywww and all file under it with the command:</p>
            <code-block>restorecon -Rv /mywww</code-block>
        </step>
        <step>
            <p>Open http://[your host IP address] on a browser again. Do you see your html page?</p>
            <tip>Yes</tip>
        </step>
    </procedure>
</chapter>
<chapter title="Cheat Sheet" id="csc325-workshop-summary">
    <deflist type="medium">
        <def title="useradd" >
        </def>
        <def title="groupadd" >
        </def>
        <def title="usermod" >
        <p><code>-G</code>: add group</p>
        </def>
        <def title="groupmod" >
        </def>
        <def title="id" >
        </def>
        <def title="su - [username]" >
        </def>
        <def title="passwd" >
        </def>
        <def title="chage" >
        </def>
    </deflist>
</chapter>

