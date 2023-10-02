# Root Signature
---
**root Signature** 란 일종의 계약서 / 결제로 이해하면 편하다. <br>
CPU의 일을 GPU에 외주에 준다 가정하면, RAM 상의 데이터를 GPU에 전달하는 계약서가 **root Signature**이 된다. <br>
Shader Update 시 Pipline State 에 Root Signature 가 포함되어 업데이트 된다. 즉, Register 상의 할당한 data 블록을 어떻게 활용할 것 인지에 대한 기술이다. ***(# Root Signature 에 기술하는 것과 실제 데이터를 할당하는 것은 별개의 작업임에 유의)***
## An empty root signature
비어있는 root signature 은 입력 어셈블러만 사용하는 간단한 상황에서 사용된다.

## One constant
![image](https://github.com/RushBsite/DirectX/assets/28249906/74349156-0c28-47a9-a34a-14d69af3e153)<br>
기본적인 root signature 은 외부 코드에서 지칭하는 번호 : `API bind slot`, 레지스터 번호: `HLSL bind slot`, 그리고 root constant 로 구성된다. 

## root Constant Buffer View 를 사용
> <p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/0887f78b-e455-441f-a9d4-fce487c36912"></p>
> <p align="center"><em>two root constants & a root CBV</em></p>

> ``2.Constant Buffer`` 예제에서 사용하던 방식으로 `Root Descriptor`(View)를 나타낸 그림이다. 테이블에 내용이 있진 않지만 내용물의 위치를 나타낸다.
> <p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/583e7ece-71ae-46bc-acc1-057e659c6f20"></p>
> 유의할 사항은 메모리에서 버퍼(View)로 복되는 것은 바로 일어나지만, register의 블록과 바인딩 되는 것은 cmd queue의 명령이 종료된 이후의 일이라는 것이다.<br>
> 따라서 memcpy 수만큼의 버퍼가 존재하지 않으면, 결과가 중복되어 마지막의 결과만 출력되는 현상이 일어난다.

## Binding descriptor tables
><p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/eb3cdaed-cea0-44a2-9c43-103f2ebe5170"></p>
> <p align="center"><em>one root constant & two desc. tables & a root CBV </em></p>
>상기한 이미지에서 확인 가능하듯, root Signature의 작성방법은 크게 3가지로 나뉜다. <br>
>1. 상수 <br>
>2. root CBV <br>
>3. desc.table <br>
>register의 용량이 매우 한정적(64DWORD)이기 때문에, 2와 3의 방법이 쓰이는것.<br>
><p align="center"><img src="https://github.com/RushBsite/DirectX/assets/28249906/a9e62430-8a3f-4619-b223-89f28215c019"></p>
>그림에서 확인가능하듯이 desc.table 방식은 최상위 테이블의 description이 View의 위치를 나타내는 구조이다.
![image](https://github.com/RushBsite/DirectX/assets/28249906/a27d160d-b64b-4148-90cc-4d56fe160312)
> root CBV 방식의 단점과 동일하게 복사하는 만큼 동일한 양의 view 가형성되어야 하는 문제가 역시 존재한다.<br>
> 최초 root Signature 기술시, table 을 사용한 코드로 작성했기 때문에, 최종형은 다음과 같다. (TableDescriptor Heap)
> ![image](https://github.com/RushBsite/DirectX/assets/28249906/4f5ff1e1-9476-452e-b7b0-333e145c2cf2)


## Sample Code
```Cpp
// desc heap 의 size와 group count를 사용함에 유의
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
