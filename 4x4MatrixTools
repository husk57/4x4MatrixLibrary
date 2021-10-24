local matrix = {}
matrix.__index = matrix
 
--[[compData is 4x4 2 dimensional array]]--
function matrix.new(inheritedFromVector, compData)
	if not compData and not inheritedFromVector then
		compData = {
			{1,0,0,0};
			{0,1,0,0};
			{0,0,1,0};
			{0,0,0,1}
		}
	end
	if inheritedFromVector then
 
		if typeof(compData) == "Vector3" then
			compData = {
				{compData.X,0,0,0};
				{compData.Y,0,0,0};
				{compData.Z,0,0,0};
				{1,0,0,0}
			}
		else
			compData = 
				{{0,0,0,0};
			{0,0,0,0};
			{0,0,0,0};
			{1,0,0,0}}
		end
	end
	return setmetatable(compData, matrix)
end
 
--[[convert the homogonous coordinate vector to a 3 dimensional roblox vector]]--
function matrix:m2v3()
	return Vector3.new(self[1][1]/self[4][1], self[2][1]/self[4][1], self[3][1]/self[4][1])
end
 
--[[create a rotational matrix, axis is a string of X, Y, or Z]]--
function matrix:rotate(axis, step)
	if axis == "X" then
		local mtx = {
			{1,0,0,0};
			{0, math.cos(step), -math.sin(step), 0};
			{0,math.sin(step), math.cos(step), 0};
			{0,0,0,1}
		}
		return matrix.new(false, mtx)
	end
	if axis == "Y" then
		local mtx = {
			{math.cos(step),0,math.sin(step),0};
			{0, 1,0, 0};
			{-math.sin(step),0, math.cos(step), 0};
			{0,0,0,1}
		}
		return matrix.new(false, mtx)
	end
	if axis == "Z" then
		local mtx = {
			{math.cos(step),-math.sin(step),0,0};
			{math.sin(step), math.cos(step),0, 0};
			{0,0,1, 0};
			{0,0,0,1}
		}
		return matrix.new(false, mtx)
	end
end
 
--[[create a perspective matrix, n is near clipping plane and f is far clipping plane]]--
function matrix:perspective(fov,n,f)
	--https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/building-basic-perspective-projection-matrix
	local s = 1/math.tan(fov*0.00872664625998)
	local mtx = {
		{s,0,0,0};
		{0,s,0,0};
		{0,0,-f/(f-n),-1};
		{0,0,-(f*n)/(f-n), 0}
	}
	return matrix.new(false, mtx)
end
 
--[[x,y,z are the scale factors for each axis]]
function matrix:scale(x,y,z)
	local mtx = {
		{x,0,0,0};
		{0,y,0,0};
		{0,0,z,0};
		{0,0,0,1}
	}
	return matrix.new(false, mtx)
end
 
local function randomUnitVector()
	local sqrt = math.sqrt(-2 * math.log(math.random()))
	local angle = 2 * math.pi * math.random()
 
	return Vector3.new(
		sqrt * math.cos(angle),
		sqrt * math.sin(angle),
		math.sqrt(-2 * math.log(math.random())) * math.cos(2 * math.pi * math.random())
	).Unit
end
 
--[[lookat matrix, parameters are roblox vector3s]]--
function matrix:lookAt(eye, target)
	local forward = (eye-target).Unit
	local right = Vector3.new(0,1,0):Cross(forward)
	local up = forward:Cross(right)
	local coordSpace = matrix.new(false, {
		{right.X, right.Y, right.Z, 0};
		{up.X, up.Y, up.Z, 0};
		{forward.X, forward.Y, forward.Z, 0};
		{0,0,0,1}
	})
	local invertedCameraPosition = matrix.new(false, {
		{1,0,0,-eye.X};
		{0,1,0,-eye.Y};
		{0,0,1,-eye.Z};
		{0,0,0,1}
	})
	return coordSpace*invertedCameraPosition
end
 
--[[perform matrix multiplication]]--
matrix.__mul = function(m1, m2)
	local mtx = {}
	for i = 1,#m1 do
		mtx[i] = {}
		for j = 1,#m2[1] do
			local num = m1[i][1] * m2[1][j]
			for n = 2,#m1[1] do
				num = num + m1[i][n] * m2[n][j]
			end
			mtx[i][j] = num
		end
	end
	return matrix.new(false, mtx)
end
 
return matrix
