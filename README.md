# spec-check windows



    "=== CPU ==="
    Get-CimInstance Win32_Processor | Select-Object Name, NumberOfCores, NumberOfLogicalProcessors, @{Name="MaxSpeed(GHz)";Expression={[math]::Round($_.MaxClockSpeed/1000,2)}}
    
    "=== RAM ==="
    Get-CimInstance Win32_PhysicalMemory | Select-Object @{Name="Capacity(GB)";Expression={[math]::Round($_.Capacity/1GB,1)}}, @{Name="MemoryType";Expression={
        switch ($_.MemoryType) {
            20 { "DDR" }
            21 { "DDR2" }
            24 { "DDR3" }
            26 { "DDR4" }
            29 { "DDR5" }
            default { "Unknown ($($_.MemoryType))" }
        }
    }}
    
    "=== Storage ==="
    Get-PhysicalDisk | Select-Object FriendlyName, @{Name="Size(GB)";Expression={[math]::Round($_.Size/1GB,1)}}, MediaType, BusType, @{Name="IsNVMe";Expression={if ($_.BusType -eq 'NVMe') {'Yes'} else {'No'}}}
    
    
    "=== GPU ==="
    Get-CimInstance Win32_VideoController | Select-Object Name, @{Name="VRAM(GB)";Expression={[math]::Round($_.AdapterRAM/1GB,1)}}
