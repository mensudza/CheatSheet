# CSC325 Security

<chapter title="Linux Workshop" id="csc325-workshop">
   <p>This section is a review of three labs about managing system with Linux</p>
   <chapter title="Workshop#1" id="csc325-workshop1" collapsible="true">

<procedure title="" id="procedure-id">
   <p> 
            <control>Instruction:</control> From a given Linux host, each student must 
            manage user accounts and passwords by using the commands <shortcut>useradd</shortcut>, 
            <shortcut>groupadd</shortcut>, <shortcut>usermod</shortcut>, <shortcut>groupmod</shortcut>, 
            <shortcut>id</shortcut>, <shortcut>su - [username]</shortcut>, <shortcut>passwd</shortcut> and 
            <shortcut>chage</shortcut> as the following criteria:
    </p>
   <step>
<p>Create a user “student01” including its home directory.</p>
<code-block>useradd student01</code-block>
</step>

   <step>
<p>Create a group “student” with GID = 10000.</p>
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
</step>



</procedure>


   </chapter>

<chapter title="Linux Cheat Sheet" id="csc325-workshop-summary" collapsible="true">
    
|   **Command**   | **Description** | **Optional** |
|:---------------:|:-----------:|:--------:|
|     useradd     |             |          |
|    groupadd     |             |          |
|     usermod     |             |          |
|    groupmod     |             |          |
|       id        |             |          |
| su - [username] |             |          |
|     passwd      |             |          |
|      chage      |             |          |

</chapter>

</chapter>