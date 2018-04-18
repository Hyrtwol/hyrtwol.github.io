# C# Code

[System.Diagnostics.DebuggerDisplayAttribute](https://msdn.microsoft.com/en-us/library/x810d419.aspx)

    [StructLayout(LayoutKind.Sequential, Pack = 4)]
    [DebuggerDisplay("X: {X}, Y: {Y}, Z: {Z}")]
    public struct Vector3
    {
        public float X, Y, Z;
    } 
