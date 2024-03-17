# Linux Workshop


   <p>This section is a review of three labs about managing system with Linux</p>
   <chapter title="Workshop#1" id="csc325-workshop1">

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

