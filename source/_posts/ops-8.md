---
title: spirent 测试
date: 2019-05-21 10:40:07
tags:
- ops
author: 相飞
comments:
- true
categories:
- spirent
- tcl

---

### spirent 流量测试


#### spirent 主流程 
+ step up a communication between your pc and your spirent testcenter chassis
+ prepare DUT/SUT
+ connect spirent testcenter chassis to DUT/SUT
+ initialize spirent testcenter api to establish the object set context
+ create project objects and set project attributes
+ create port objects and set port attributes
+ create streamblock objects and set streamblock attributes
+ setup spirent generator and analyzer  for traffic support
+ establish the software connection from your pc to the spirent testcase chassis
+ start test
+ review/retrive the test results
+ cleanup  after finish

#### 监控项
+ 吞吐量
+ frame lost
+ 延时时间

#### tcl demo

```
###############################################################################
#
#                     2544 Spirent TestCenter Lingo Example
#                         by Spirent Communications
#
#   Date: May 11, 2011
# Author: Matthew Jefferson - matt.jefferson@spirent.com
#
# Description: An example of how to create and execute an RFC2544 throughput
#              test using Spirent TestCenter Lingo.
#
#
# Topology:
#   There are three ports (IPv4) in a full mesh.
#   The IP addressing is:
#       UUT        STCPort
#   192.85.10.1 - 192.85.10.2   Port1
#   192.85.11.1 - 192.85.11.2   Port2
#   192.85.12.1 - 192.85.12.2   Port3
#
###############################################################################

###############################################################################
# Copyright (c) 2011 SPIRENT COMMUNICATIONS OF CALABASAS, INC.
# All Rights Reserved
#
#                SPIRENT COMMUNICATIONS OF CALABASAS, INC.
#                            LICENSE AGREEMENT
#
#  By accessing or executing this software, you agree to be bound by the terms
#  of this agreement.
#
# Redistribution and use of this software in source and binary forms, with or
# without modification, are permitted provided that the following conditions
# are met:
#  1. Redistribution of source code must retain the above copyright notice,
#     this list of conditions and the following disclaimer.
#  2. Redistribution's in binary form must reproduce the above copyright notice.
#     This list of conditions and the following disclaimer in the documentation
#     and/or other materials provided with the distribution.
#  3. Neither the name SPIRENT, SPIRENT COMMUNICATIONS, SMARTBITS, nor the names
#     of its contributors may be used to endorse or promote products derived
#     from this software without specific prior written permission.
#
# This software is provided by the copyright holders and contributors [as is]
# and any express or implied warranties, including, but not limited to, the
# implied warranties of merchantability and fitness for a particular purpose
# are disclaimed. In no event shall the Spirent Communications of Calabasas,
# Inc. Or its contributors be liable for any direct, indirect, incidental,
# special, exemplary, or consequential damages (including, but not limited to,
# procurement of substitute goods or services; loss of use, data, or profits;
# or business interruption) however caused and on any theory of liability,
# whether in contract, strict liability, or tort (including negligence or
# otherwise) arising in any way out of the use of this software, even if
# advised of the possibility of such damage.
#
###############################################################################


set ::env(STC_LOG_OUTPUT_DIRECTORY) [pwd]

set api_path /opt/tools/Spirent_TestCenter_4.81/Spirent_TestCenter_Application_Linux
lappend ::auto_path $api_path
package require SpirentTestCenter


###############################################################################
####
####    Global Variables
####
###############################################################################

set trafficduration   10            ;# In seconds.
set startingipaddress 10.1.1.2      ;# This IP address will be given to the first port in the "locationlist".
                                    ;# Each additional port will be incremented by 0.0.1.0.
set startinggateway   10.1.1.1   ;# This Gateway address will be given to the first port in the "locationlist".
                                    ;# Each additional port will be incremented by 0.0.1.0.

# The mapping of STC ports to the actual hardware locations.
set locationlist {//192.168.230.220/2/7
                  //192.168.230.220/2/8}

set framesizelist "64 1518"

###############################################################################
####
####    Procedures
####
###############################################################################

proc debugGUI { } {
    # This procedure displays a Tk-based object browser, allowing the user to
    # view the API's entire object tree.
    package require stclib

    stclib::gdbg::stcdebug on
    stclib::gdbg::stchelp on
    stclib::gdbg::start
    stclib::gdbg::update -pause true -label 1
    stclib::gdbg::stop

    return
}

#==============================================================================
proc createReport { databasefilename reportfilename template } {
    # Use the CLI interface for the results reporter to generate the desired report.
    set rrpath [file join $::api_path "results_reporter"]

    if { $::tcl_platform(platform) eq "unix" } {
        set rrexecutable "ResultsReporterCLI.sh"
    } else {
        set rrexecutable "ResultsReporterCLI.bat"
    }

    # The results reporter needs to be lauched from its own directory.
    set currentpath [pwd]
    cd $rrpath

    # Try and automatically determine the report type (pdf, csv or html) from the
    # report filename extension.
    # Get the extension and strip off the leading ".".
    set reporttype [string range [file extension $reportfilename] 1 end]

    # This switch really only makes sure that the reporttype is valid.
    switch [string tolower $reporttype] {
        csv      -
        csv-tree { set extension "csv" }
        html     { set extension "html" }
        pdf-tree { set extension "pdf" }
        jpeg     { set extension "jpeg" }
        default {
            set reporttype "pdf"
            set extension  "pdf"
        }
    }

    set template       "[file rootname $template].rtp"
    set templatepath   [file join $rrpath templates/$template]
    set database       [file normalize [file join $currentpath $databasefilename]]
    set outputfilename [file normalize [file join $currentpath $reportfilename]]

    exec >&@ stdout [file join $rrpath $rrexecutable] -f $reporttype \
                                                      -o $database \
                                                      -d $outputfilename \
                                                      -t $templatepath

    cd $currentpath

    return
}



###############################################################################
####
####    Main
####
###############################################################################

# Create the root project object
puts "Creating project ..."
set project [stc::create project]       ;# Storing the project handle isn't necessary, but I'll do it here.

stc::config automationoptions -logTo             "stcapi.log" \
                              -logLevel          INFO         \
                              -suppresstclerrors false

## Construct a list of ports in the config array.
#foreach name [array names config] {
#    regexp {//[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}/[0-9]{1,2}/[0-9]{1,2}} $name port
#    lappend config(port.list) $port
#}

# Create the ports and assign them a hardware location.
foreach port $locationlist {
    set config($port) [stc::create port -under          [stc::get system1 -children-project] \
                                        -location       $port                                \
                                        -usedefaulthost false]

    # If you want to create a host for each port indiviually, you can do it here.

} ;# End foreach port

# You have the option to create the hosts individually, or by using the "wizard" (DeviceGenConfigExpand).
# If you create the hosts individually, you have more flexibility in assigning the addressing to the hosts.
# However, my emulated host addresses are in a single, contiguous sequence, so I'm going to use the wizard.
# The following code will create a "Host" object on each port in the configuration.
stc::perform DeviceGenConfigExpand -DeleteExisting yes                                     \
                                   -HostGenParams [list                                    \
                                        -Count 1                                           \
                                        -Port  [stc::get system1.project -children-port]   \
                                        -DeviceGenEthIIIfParams.SrcMac "00:10:95:00:00:01" \
                                        -DeviceGenIpv4IfParams [list                       \
                                            Addr        $startingipaddress                 \
                                            Gateway     $startinggateway                   \
                                            AddrStep    0.0.1.0                            \
                                            GatewayStep 0.0.1.0]]

# This is an alternate way of specifying the same values.
#stc::perform DeviceGenConfigExpand -DeleteExisting      yes \
#                                   -HostGenParams.Count 1 \
#                                   -HostGenParams.Port  [stc::get system1.project -children-port]   \
#                                   -HostGenParams.DeviceGenEthIIIfParams.SrcMac "00:10:95:00:00:01" \
#                                   -HostGenParams.DeviceGenIpv4IfParams.Addr    $startingipaddress



# This is the Lingo command to set up the 2544 throughput test.
puts "Set up the RFC2544 throughput test..."
stc::perform Rfc2544SetupThroughputTestCommand -TrafficPattern MESH      \
                                               -NumOfTrials    1         \
                                               -Duration       10        \
                                               -FrameSizeList  $framesizelist \
                                               -SearchMode     BINARY    \
                                               -RateLowerLimit 50        \
                                               -RateUpperLimit 100       \
                                               -RateInitial    100       \
                                               -Resolution     1         \
                                               -LearningMode   NONE


# Save this configuration to a file. This can be loaded into the GUI if needed.
stc::perform SaveToTcc -config system1 -filename [file normalize "currentconfig.tcc"]


# Connect to the hardware...
stc::perform attachPorts -portList    [stc::get system1.project -children-port] \
                         -autoConnect TRUE

# Apply configuration.
puts "Apply configuration..."
stc::apply

puts "Starting the sequencer..."
stc::perform SequencerStart

# Wait for sequencer to finish
puts "Waiting for the sequencer to complete...this may take a little while."
stc::waituntilcomplete

puts "The test has completed...Saving results..."
# Determine what the results database filename is...
set resultsdb [stc::get system1.project.TestResultSetting -CurrentResultFileName]


puts "The results database is '$resultsdb'"


if {0} {
    puts "Exporting the results to CSV...the file will be in the same location as the results database."
    stc::perform ExportDbResults -TemplateUri    "templates/Rfc2544ThroughputStats.rtp" \
                                 -ResultDbFile   $resultsdb                             \
                                 -Format         "BINARY"                               \
                                 -ResultFileName "Demo"

} else {
    createReport $resultsdb test_report.pdf Rfc2544ThroughputStats
}


puts "Done!"


```

