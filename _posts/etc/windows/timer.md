전반적으로 FPS에 대한 영향과 관련하여 내가 본 차이는 너무 작아서 오차 범위로만 좁힐 수 있지만 이것이 내가 내린 결론입니다.

타이머

고정밀 이벤트 타이머는 BIOS에서 활성화 또는 비활성화됩니다.
bcdedit는 상승된 명령 프롬프트를 통해 구성됩니다.

TSC+LAPIC - 대기 시간이나 끊김 현상이 발생하지 않는 것 같고, 입력 및 출력이 원활합니다 (일반적으로 기본값).
고정밀 이벤트 타이머: 비활성화됨
bcdedit /deletevalue useplatformclock

LAPIC - 끊김 현상이 발생하지만 대기 시간이 없는 것 같습니다.
높음 정밀 이벤트 타이머: 비활성화됨
bcdedit /set useplatformclock true

TSC+HPET - 대기 시간 및 버벅 거림 을 유발하는 것 같습니다.
고정밀 이벤트 타이머: 활성화됨
bcdedit /deletevalue useplatformclock

HPET - 대기 시간이 발생하는 것 같지만 스터터가 발생하지 않고 입력 및 출력이 매우 매끄럽습니다.
고정밀 이벤트 타이머: 활성화됨
bcdedit /set useplatformclock true

다음은 아마도 방금 만든 것 같습니다.

TSC 타이머는 각 프로세서에 상대적이고 LAPIC 타이머는 시스템 버스에 상대적이고 HPET는 외부에 있다고 생각합니다. TSC 타이머는 동기화 상태를 유지하지 않기 때문에 단독으로 사용할 수 없으므로 TSC 대신 HPET 및 LAPIC가 사용되거나 TSC 타이머의 매우 낮은 대기 시간을 문제 없이 활용할 수 있도록 함께 사용됩니다. 동기화되지 않습니다. 나는 TSC와 LAPIC이 동일한 클럭/크리스털에서 시간을 파생하므로 서로 잘 맞지만 HPET는 그렇지 않으며 HPET를 사용하면 대기 시간과 끊김 문제가 발생하는 이유 중 일부일 것입니다. 모든 것과 동기화합니다. 정말 높은 주파수의 목적은 성능 향상이 아니라 HPET의 동기화 및 대기 시간 문제를 완화하려는 시도일 수도 있습니다.

타임 스탬프 카운터 동기화 정책

이것을 고급으로 설정하면 효과가 없거나 버벅거림이 발생했습니다. 지금 내가 이해한 바에 따르면 Windows는 이미 이에 대한 최상의 설정을 선택하므로 그대로 두어야 합니다.

설정을 강화하려면:
bcdedit /set tscsyncpolicy Enhanced

강제 설정을 제거하려면:
bcdedit /deletevalue tscsyncpolicy

동적 타이머 틱

둘 다 눈에 띄는 효과는 없었지만 이전에 문제를 일으킨 것으로 문서화되어 있으며 실제 이점도 제공하지 않으며 그렇지도 않습니다. Windows 7 또는 이전 버전에서 사용되거나 필요하므로 비활성화하기로 선택했습니다.

비활성화하려면:
bcdedit /set disabledynamictick yes

활성화하려면:
bcdedit /deletevalue disabledynamictick
