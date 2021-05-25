macroScript SphericalNormalGenerater category:"MazaCustom"
(
	rollout HelpWindow "Help_Note" width:384 height:224
	(
		GroupBox 'grp4' "Generate from a help point" pos:[8,8] width:368 height:88 align:#left
		label 'lbl6' "The target mesh vertex normal will be generate as the selected help point towards to each vertex's normalized vector." pos:[24,30] width:336 height:58 align:#left
		
		GroupBox 'grp5' "Generate form reference mesh" pos:[8,112] width:368 height:72 align:#left
		label 'lbl8' "Find three nearest vertexes of reference mesh.Lerp these vertex normal with distance as the target mesh vertex normal." pos:[24,136] width:336 height:48 align:#left
		label 'lbl11' "Mazarine Skygem" pos:[272,184] width:96 height:16 align:#left
		label 'lbl12' "2021.05.09" pos:[288,200] width:72 height:16 align:#left
	)



	rollout TransportVertsNormal "Spherical Vertex Normal Generate" width:216 height:450
	(
		--预先申明个布尔值，防止关闭脚本的时候误删模型
		Local isTargetMeshNew = false
		Local isHelpPointNew = false
		Local isRefMeshNew = false
		Local isReadyOrNot = false
		
		--UI-Functions
		GroupBox 'modeGroup' "Mode" pos:[8,8] width:200 height:64 align:#left
		radiobuttons 'transportMod' "" pos:[16,32] width:135 height:32 enabled:true labels:#("Generate with a help-point", "trans from reference mesh") default:2 columns:1 align:#left
			--transportMod.state
			-- 1 = 由辅助点生成法线
			-- 2 = 参考模型传递法线
		
		GroupBox 'pickMeshGroup' "Pick Obj" pos:[8,88] width:202 height:216 align:#left
		
		label 'targetMeshName' "None" pos:[16,152] width:184 height:16 enabled:true align:#right
		label 'helpPointName' "None" pos:[16,216] width:184 height:16 enabled:true align:#left
		label 'refMeshName' "None" pos:[16,280] width:184 height:16 enabled:true align:#left

		--拾取模型的检测，应该只要是拾取模型都可以用这个function
		function MeshTest i_obj = 
		(
			(classOf i_obj) == Editable_mesh \
			or (classOf i_obj) == Editable_Poly \
			or (classOf(classOf i_obj)) == GeometryClass
		)
		
		function PointTest i_obj = classOf i_obj == Point
		
		--拾取目标模型按钮
		pickbutton 'pickTargetMeshButton' "Pick target mesh" pos:[16,112] width:184 height:32 \
					 enabled:true align:#left filter:MeshTest
		
		pickbutton 'pickHelpPointButton' "Pick help point" pos:[16,176] width:184 height:32 \
					enabled:false align:#left filter:PointTest
		
		pickbutton 'pickRefMeshButton' "Pick reference mesh" pos:[16,240] width:184 height:32 \
					enabled:true align:#left filter:MeshTest
		
		progressBar 'progress' "ProgressBar" pos:[8,316] width:200 height:16 value:0 align:#left
		
		label 'progressText' "0/0" pos:[8,340] width:184 height:16 enabled:true align:#right
		
		button 'helpMessage' "Help note" pos:[40,365] width:136 height:32 enabled:true toolTip:"" align:#left
		
		button 'generateNormal' "Generate" pos:[40,405] width:136 height:32 enabled:false toolTip:"" align:#left

		
		
		--拾取 模型、辅助点 按钮 启用、禁用 切换
		
		function isGenerateButtonReady = 
		(
			if $tmpTargetMesh_Maza != undefined and isTargetMeshNew do \
			case of
			(
				(transportMod.state == 1) : if $tmpHelpPoint_Maza != undefined and isHelpPointNew then isReadyOrNot = true else isReadyOrNot = false
				(transportMod.state == 2) : if $tmpRefMesh_Maza != undefined and isRefMeshNew then isReadyOrNot = true else isReadyOrNot = false
			)
		)
		
			on pickTargetMeshButton picked obj do
		(
			if obj != undefined do
			(
				if ($tmpTargetMesh_Maza == undefined or isTargetMeshNew == true)and obj!=$tmpTargetMesh_Maza \
					then 
					(
						--先检查如果模型不为空就删了模型
						if $tmpTargetMesh_Maza != undefined do delete $tmpTargetMesh_Maza
							
						--拷贝出一个新的模型来使用，检查是否是可编辑网格，如果不是转换一下
						copy obj name:"tmpTargetMesh_Maza" wireColor:[127,127,255]
						if classOf $tmpTargetMesh_Maza != Editable_mesh \
							do convertTo $tmpTargetMesh_Maza Editable_mesh
						targetMeshName.text = obj.name
						
						--更新布尔值，关闭脚本的时候删除这个新创建的模型
						isTargetMeshNew = true
						isGenerateButtonReady()
						generateNormal.enabled = isReadyOrNot
						
					)
					else messageBox "tmpRefMesh_Maza is occupied"
			)
		)

		--下面这段逻辑和上面几乎一样，就不重复加注释了，删除了可编辑网格转换这一步
		on pickHelpPointButton picked obj do
		(
			if obj != undefined do
			(
				if ($tmpHelpPoint_Maza == undefined or isHelpPointNew == true) and obj != $tmpHelpPoint_Maza \
					then 
					(
						if $tmpHelpPoint_Maza != undefined do delete $tmpHelpPoint_Maza
						copy obj name:"tmpHelpPoint_Maza" wireColor:[127,127,255]
						helpPointName.text = obj.name
						isHelpPointNew = true	
						isGenerateButtonReady()
						generateNormal.enabled = isReadyOrNot
					)
					else messageBox "tmpRefMesh_Maza is occupied"

			)
		)
		
		on pickRefMeshButton picked obj do
		(
			if obj != undefined do
			(
				if ($tmpRefMesh_Maza == undefined or isRefMeshNew == true)and obj!=$tmpRefMesh_Maza \
					then 
					(
						--先检查如果模型不为空就删了模型
						if $tmpRefMesh_Maza != undefined do delete $tmpRefMesh_Maza
							
						--拷贝出一个新的模型来使用，检查是否是可编辑网格，如果不是转换一下
						copy obj name:"tmpRefMesh_Maza" wireColor:[127,127,255]
						if classOf $tmpRefMesh_Maza != Editable_mesh \
							do convertTo $tmpRefMesh_Maza Editable_mesh
						refMeshName.text = obj.name
						
						--更新布尔值，关闭脚本的时候删除这个新创建的模型
						isRefMeshNew = true	
						isGenerateButtonReady()
						generateNormal.enabled = isReadyOrNot
					)
					else messageBox "tmpRefMesh_Maza is occupied"
			)
		)

		on helpMessage pressed do CreateDialog HelpWindow

		on generateNormal pressed do
		(
			Local totalVert = getNumVerts $tmpTargetMesh_Maza
			case of 
			(
				(transportMod.state == 1) : 
					(
						for i = 1 to totalVert by 1 do
						(
							NV = normalize((getVert $tmpTargetMesh_Maza i) - $tmpHelpPoint_Maza.position)
							setNormal $tmpTargetMesh_Maza i NV
							
							progress.value = 100*i/totalVert
							progressText.text = (i as String) + " / " + (totalVert  as String)
						)
						update $tmpTargetMesh_Maza
						copy $tmpTargetMesh_Maza name:(targetMeshName.text + "_reGenNormal")
						
						--重制拾取对象和按钮
						delete $tmpTargetMesh_Maza
						isTargetMeshNew = false
						targetMeshName.text = "None"
						delete $tmpHelpPoint_Maza
						isHelpPointNew = false
						helpPointName.text = "None"
						
						isGenerateButtonReady()
						generateNormal.enabled = isReadyOrNot
					)
				(transportMod.state == 2) : 
					(
						Local p0
						Local p1
						Local p2
						Local p3
						
						Local refMeshVertCount = getNumVerts $tmpRefMesh_Maza
						
						for u = 1 to totalVert by 1 do
						--for u = 1 to 1 by 1 do
						(
							Local currentTargetPoint = getVert $tmpTargetMesh_Maza u
							
							--找到最近的三个点
							for u = 1 to refMeshVertCount by 1 do
							(
								--当前拾取的参考模型的顶点
								--和当前目标模型顶点到参考模型顶点的距离
								Local o = getVert $tmpRefMesh_Maza u
								Local d = distance o currentTargetPoint
								p0 = #(u , d)
								
								--收集距离最近的三个顶点并依距离从小到大排序
								case of
								(
									(u == 1): p1 = p0
									
									(u == 2): 
									(
										if (d < p1[2]) then 
										(
											p2 = p1
											p1 = p0
										)else p2 = p0
									)
									
									(u == 3): 
									(
										if (d < p2[2]) then
										(
											if (d < p1[2]) then
											(
												p3 = p2
												p2 = p1
												p1 = p0
											)else(
												p3 = p2
												p2 = p0
											)
										)else(
											p3 = p0
										)
									)
									
									(u > 3):
									(
										if (d < p3[2]) then
										(
											if (d < p2[2]) then
											(
												if (d < p1[2]) then
												(
													p3 = p2
													p2 = p1
													p1 = p0
												)else(
													p3 = p2
													p2 = p0
												)
											)else(
												p3 = p0
											)
										)
									)
									
								)
							)
						--计算法线
						
						Local lerpA = p1[2] / (p1[2] + p2[2])
						Local tmp_N = lerpA*(getNormal $tmpRefMesh_Maza p1[1]) + (1 - lerpA)*(getNormal $tmpRefMesh_Maza p2[1])
						Local tmp_P = lerpA*(getVert $tmpRefMesh_Maza p1[1]) + (1 - lerpA)*(getVert $tmpRefMesh_Maza p2[1])
						lerpA = p3[2] / ((distance tmp_P currentTargetPoint) + p3[2])
						tmp_N = lerpA * (getNormal $tmpRefMesh_Maza p3[1]) + (1 - lerpA) * tmp_N
						
						setNormal $tmpTargetMesh_Maza u tmp_N
						
						progress.value = 100*u/totalVert
						progressText.text = (u as String) + " / " + (totalVert  as String)
						)
						
						update $tmpTargetMesh_Maza
						copy $tmpTargetMesh_Maza name:(targetMeshName.text + "_reGenNormal")
						
						--重制拾取对象和按钮
						delete $tmpTargetMesh_Maza
						isTargetMeshNew = false
						targetMeshName.text = "None"
						delete $tmpRefMesh_Maza
						isRefMeshNew = false
						refMeshName.text = "None"
						
						isGenerateButtonReady()
						generateNormal.enabled = isReadyOrNot
					)
			)

		)
		
		
		on transportMod changed state do
		(
			case of 
			(
				(transportMod.state == 1) : 
					(
						pickHelpPointButton.enabled = true
						pickRefMeshButton.enabled = false
					)
				(transportMod.state == 2) : 
					(
						pickHelpPointButton.enabled = false
						pickRefMeshButton.enabled = true
					)
			)
			isGenerateButtonReady()
			generateNormal.enabled = isReadyOrNot
		)

		--关闭GUI时候的清理生成的Obj
		on transportVertsNormal close do 
		(
			if $tmpTargetMesh_Maza != undefined and isTargetMeshNew then delete $tmpTargetMesh_Maza
			if $tmpHelpPoint_Maza != undefined and isHelpPointNew then delete $tmpHelpPoint_Maza
			if $tmpRefMesh_Maza != undefined and isRefMeshNew then delete $tmpRefMesh_Maza
		)
)
	
	CreateDialog TransportVertsNormal
)


