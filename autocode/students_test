package coverage

import (
	"os"
	"testing"
	"time"
	"fmt"
	"github.com/stretchr/testify/assert"
)

// DO NOT EDIT THIS FUNCTION
func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

// PEOPLE TESTS

func TestPeople_Len(t *testing.T) {
	tests := []struct {
		name string
		p People
		want int
	}{
		{
			name: "empty",
			p: People{},
			want: 0,
		},
		{
			name: "one",
			p: People{Person{}},
			want: 1,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			assert.Equal(t, tt.p.Len(), tt.want)
		})
	}
}

func TestPeople_Less(t *testing.T) {
	type args struct {
		i int
		j int
	}
	peoples := People{
		Person{"abc", "abc", time.Date(2000, 1, 01, 00, 00, 00, 00, time.UTC)},
		Person{"bce", "bce", time.Date(2001, 1, 01, 00, 00, 00, 00, time.UTC)},
		Person{"bce", "bce", time.Date(2000, 1, 01, 00, 00, 00, 00, time.UTC)},
		Person{"abc", "bce", time.Date(2000, 1, 01, 00, 00, 00, 00, time.UTC)},
	}
	tests := []struct {
		name string
		p    People
		args args
		want bool
	}{
		{
			"first less then second", 
			peoples,
			args{0,1},
			false,
		},
		{
			"second less then first", 
			peoples,
			args{1,0},
			true,
		},
		{
			"same dates different names", 
			peoples,
			args{0,2},
			true,
		},
		{
			"same dates same names different last names", 
			peoples,
			args{0,3},
			true,
		},
		{
			"same dates same names same last names", 
			peoples,
			args{0,0},
			false,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			assert.Equal(t, tt.p.Less(tt.args.i, tt.args.j), tt.want)
		})
	}
}

func TestPeople_Swap(t *testing.T) {
	type args struct {
		i int
		j int
	}
	peoples := People{
		Person{"abc", "abc", time.Date(2000, 1, 01, 00, 00, 00, 00, time.UTC)},
		Person{"bce", "bce", time.Date(2001, 1, 01, 00, 00, 00, 00, time.UTC)},
	}
	peoplest1 := People{
		Person{"bce", "bce", time.Date(2001, 1, 01, 00, 00, 00, 00, time.UTC)},
		Person{"abc", "abc", time.Date(2000, 1, 01, 00, 00, 00, 00, time.UTC)},
	}
	tests := []struct {
		name string
		p    People
		args args
		want People
	}{
		{
			"swap test", 
			peoples,
			args{0,1},
			peoplest1,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			tt.p.Swap(tt.args.i, tt.args.j)
			assert.Equal(t, tt.p, tt.want)
		})
	}
}

///////////////////////////////////////////////////////////////////////////////////////////////////////

func TestMatrixNew(t *testing.T) {
	type args struct {
		str string
	}
	tests := []struct {
		name    string
		args    args
		want    *Matrix
		wantErr error
	}{
		{
			name:    "Positive test",
			args:    args{str: "1 2 3\n4 5 6"},
			want:    &Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			wantErr: nil,
		},
		{
			name:    "Rows of different length",
			args:    args{str: "1 2 3\n4 5 6 7"},
			want:    nil,
			wantErr: fmt.Errorf("Rows need to be the same length"),
		},
		{
			name:    "Unconvertible chars",
			args:    args{str: "A B C"},
			want:    nil,
			wantErr: assert.AnError,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			got, err := New(tt.args.str)
			if tt.wantErr == nil {
				assert.Equal(t, tt.want, got)
				assert.Nil(t, err)
			} else {
				assert.Nil(t, tt.want)
				assert.Error(t, err)
			}
		})
	}
}

func TestMatrix_Rows(t *testing.T) {
	tests := []struct {
		name string
		m    Matrix
		want [][]int
	}{
		{
			name: "Positive test",
			m:    Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: [][]int{{1, 4}, {2, 5}, {3, 6}},
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			assert.Equal(t, tt.m.Cols(), tt.want)
		})
	}
}

func TestMatrix_Cols(t *testing.T) {
	tests := []struct {
		name string
		m    Matrix
		want [][]int
	}{
		{
			name: "Positive test",
			m:    Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: [][]int{{1, 2, 3}, {4, 5, 6}},
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			assert.Equal(t, tt.m.Rows(), tt.want)
		})
	}
}

func TestMatrix_Set(t *testing.T) {
	type args struct {
		row   int
		col   int
		value int
	}
	tests := []struct {
		name string
		m    *Matrix
		args args
		want bool
	}{
		{
			name: "Positive test",
			args: args{1, 1, 10},
			m:    &Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: true,
		},
		{
			name: "negative test1",
			args: args{-1, 1, 10},
			m:    &Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: false,
		},
		{
			name: "negative test2",
			args: args{10, 1, 10},
			m:    &Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: false,
		},
		{
			name: "negative test3",
			args: args{1, -1, 10},
			m:    &Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: false,
		},
		{
			name: "negative test4",
			args: args{1, 10, 10},
			m:    &Matrix{2, 3, []int{1, 2, 3, 4, 5, 6}},
			want: false,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			assert.Equal(t, tt.m.Set(tt.args.row, tt.args.col, tt.args.value), tt.want)
		})
	}
}