# BVH file
BVH是BioVision等设备对人体运动进行捕获后产生的文件格式，其中包含角色的骨骼和肢体关节旋转数据。

## 格式
分成Hierarchy和motion两个部分。  
### Hierarchy  
这个部分包含了骨架信息，记录了人体的各个关节点是如何连接的。通常以骨盆节点为根，根据各个关节点的连接关系形成树状层次化结构。如下方例子所示，Hips为根节点，Chest、LeftUpLeg等等为其子节点。根节点以ROOT标记，其他节点以JOINT标记，End Site表示递归结束。    
在每个节点中，都记录了OFFSET, CHANNELS等信息，其含义如下：  
- OFFSET：节点相对于父节点的偏移量，三个分量分别为X,Y,Z，根据这些offset信息可以画出来起始状态下(也称为T pose)的人体骨架。  
- CHANNELS：这个信息为关节点的位移和旋转分量在motion帧向量中的顺序，在bvh文件中用欧拉角来表示关节的旋转。Channels的顺序与后续motion中每一帧向量中维度的顺序是对应的，如下面例子，每一帧的前六个维度就是ROOT的位移(x,y,z三个分量)信息和旋转(绕z,x,y轴)信息，接着第七八九个维度是子关节Chest的三个轴旋转分量，以此类推。  
- 坐标系：每个关节都有自己的局部坐标系，CHANNELS也是相对于其局部坐标系而言。在T pose中，所有坐标系的朝向一致，ROOT的坐标系与世界坐标系重合，其他关节点OFFSET表示该关节局部坐标系相对于父节点局部坐标系的位移，朝向与世界坐标系也是一致的。    
```bvh
	HIERARCHY
	ROOT Hips
	{
		OFFSET  0.00    0.00    0.00
		CHANNELS 6 Xposition Yposition Zposition Zrotation Xrotation Yrotation
		JOINT Chest
		{
			OFFSET   0.00    5.21    0.00
			CHANNELS 3 Zrotation Xrotation Yrotation
			JOINT Neck
			{
				OFFSET   0.00    18.65   0.00
				CHANNELS 3 Zrotation Xrotation Yrotation
				JOINT Head
				{
					OFFSET   0.00    5.45    0.00
					CHANNELS 3 Zrotation Xrotation Yrotation
					End Site 
					{
						OFFSET   0.00    3.87    0.00
					}
				}
			}
			JOINT LeftCollar
			{
				OFFSET   1.12    16.23   1.87
				CHANNELS 3 Zrotation Xrotation Yrotation
				JOINT LeftUpArm
				{
					OFFSET   5.54    0.00    0.00
					CHANNELS 3 Zrotation Xrotation Yrotation
					JOINT LeftLowArm
					{
						OFFSET   0.00   -11.96   0.00
						CHANNELS 3 Zrotation Xrotation Yrotation
						JOINT LeftHand
						{
							OFFSET   0.00   -9.93    0.00
							CHANNELS 3 Zrotation Xrotation Yrotation
							End Site 
							{
								OFFSET   0.00   -7.00    0.00
							}
						}
					}
				}
			}
			JOINT RightCollar
			{
				OFFSET  -1.12    16.23   1.87
				CHANNELS 3 Zrotation Xrotation Yrotation
				JOINT RightUpArm
				{
					OFFSET  -6.07    0.00    0.00
					CHANNELS 3 Zrotation Xrotation Yrotation
					JOINT RightLowArm
					{
						OFFSET   0.00   -11.82   0.00
						CHANNELS 3 Zrotation Xrotation Yrotation
						JOINT RightHand
						{
							OFFSET   0.00   -10.65   0.00
							CHANNELS 3 Zrotation Xrotation Yrotation
							End Site 
							{
								OFFSET   0.00   -7.00    0.00
							}
						}
					}
				}
			}
		}
		JOINT LeftUpLeg
		{
			OFFSET   3.91    0.00    0.00
			CHANNELS 3 Zrotation Xrotation Yrotation
			JOINT LeftLowLeg
			{
				OFFSET   0.00   -18.34   0.00
				CHANNELS 3 Zrotation Xrotation Yrotation
				JOINT LeftFoot
				{
					OFFSET   0.00   -17.37   0.00
					CHANNELS 3 Zrotation Xrotation Yrotation
					End Site 
					{
						OFFSET   0.00   -3.46    0.00
					}
				}
			}
		}
		JOINT RightUpLeg
		{
			OFFSET  -3.91    0.00    0.00
			CHANNELS 3 Zrotation Xrotation Yrotation
			JOINT RightLowLeg
			{
				OFFSET   0.00   -17.63   0.00
				CHANNELS 3 Zrotation Xrotation Yrotation
				JOINT RightFoot
				{
					OFFSET   0.00   -17.14   0.00
					CHANNELS 3 Zrotation Xrotation Yrotation
					End Site 
					{
						OFFSET   0.00   -3.75    0.00
					}
				}
			}
		}
	}
```