---
layout: page
title: Arquivos mais complexos
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e985
---

```
<?xml version="1.0"  encoding="UTF-8" ?>

<project name="testsite" basedir="." default="main">
    <property file="./build.properties" />

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="builddir" value="./build/testsite" override="true" />
    <property name="srcdir"   value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="." id="allfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${builddir}">
            <fileset refid="allfiles" />
        </copy>
    </target>

    <!-- ============================================  -->
    <!-- Target: Rebuild                               -->
    <!-- ============================================  -->
    <target name="rebuild" description="rebuilds this package">
        <delete dir="${builddir}" />
        <phingcall target="main" />
    </target>
</project>
```