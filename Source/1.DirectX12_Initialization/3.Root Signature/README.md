# Root Signature
---
## RT1
> <p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/0887f78b-e455-441f-a9d4-fce487c36912"></p>
> 내용
> <p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/583e7ece-71ae-46bc-acc1-057e659c6f20"></p>
> 내용

## RT2
><p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/eb3cdaed-cea0-44a2-9c43-103f2ebe5170"></p>
>내용
><p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/a9e62430-8a3f-4619-b223-89f28215c019"></p>
>내용

## Sample Code
```Cpp
class TableDescriptorHeap
{
public:
	void Init(uint32 count);

	void Clear();
	void SetCBV(D3D12_CPU_DESCRIPTOR_HANDLE srcHandle, CBV_REGISTER reg);
	void CommitTable();

	ComPtr<ID3D12DescriptorHeap> GetDescriptorHeap() { return _descHeap; }

	D3D12_CPU_DESCRIPTOR_HANDLE GetCPUHandle(CBV_REGISTER reg);

private:
	D3D12_CPU_DESCRIPTOR_HANDLE GetCPUHandle(uint32 reg);

private:

	ComPtr<ID3D12DescriptorHeap> _descHeap;
	uint64					_handleSize = 0;
	uint64					_groupSize = 0;
	uint64					_groupCount = 0;

	uint32					_currentGroupIndex = 0;
};
```

## Output
![2-1 TableDescriptorHeap](https://github.com/RushBsite/DirectX/assets/28249906/c21c0a76-1cd1-45ce-af36-6376487ca9ca)
